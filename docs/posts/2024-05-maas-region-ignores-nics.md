---
date: 2024-05-09T13:00:00.000+02:00
description: ''
published: true
slug: 2024-21-maas-region-ignores-nics
tags:
- MAAS
- Linux
- DevOps
time_to_read: 3
title: MAAS regiond ignores new NICs, what now?
---
## Long story short
I have an old MAAS `regiond+rackd` (the whole thing) installed from `snap` and updated over time to the fairly recent `stable/3.4` channel that now corresponds to the version `3.4.2`.

I noticed that new NICs added to the VM that runs the MAAS install never appear in the MAAS controllers -> Interfaces page. But I need it in order to connect MAAS to the new VLANs we just deployed.

First clues comes from logs. The `/var/snap/maas/common/log/rackd.log` contains these errors:
```
2024-05-09 12:00:14 twisted.internet.defer: [critical] Unhandled error in Deferred:
2024-05-09 12:00:14 twisted.internet.defer: [critical] 
	Traceback (most recent call last):
	Failure: twisted.internet.error.MulticastJoinError: (b'\xe0\x00\x00v', b'\n\x0c\n\xf8', 98, 'Address already in use')
	
2024-05-09 12:00:14 twisted.internet.defer: [critical] Unhandled error in Deferred:
2024-05-09 12:00:14 twisted.internet.defer: [critical] 
	Traceback (most recent call last):
	Failure: twisted.internet.error.MulticastJoinError: (b'\xe0\x00\x00v', b'\n\x0c\x07\xf8', 98, 'Address already in use')
	
2024-05-09 12:00:14 twisted.internet.defer: [critical] Unhandled error in Deferred:
2024-05-09 12:00:14 twisted.internet.defer: [critical] 
	Traceback (most recent call last):
	Failure: twisted.internet.error.MulticastJoinError: (b'\xe0\x00\x00v', b'\n\x0c\x13\xfa', 98, 'Address already in use')
	
2024-05-09 12:00:14 provisioningserver.rpc.clusterservice: [info] Rack controller 'rsryyc' registered (via sf-maasregion-1:pid=2275) with MAAS version 3.4.2-14353-g.5a5221d57.
2024-05-09 12:00:14 provisioningserver.rpc.clusterservice: [info] Rack controller 'rsryyc' registered (via sf-maasregion-1:pid=2275) with MAAS version 3.4.2-14353-g.5a5221d57.
2024-05-09 12:00:14 provisioningserver.rpc.clusterservice: [info] Rack controller 'rsryyc' registered (via sf-maasregion-1:pid=2275) with MAAS version 3.4.2-14353-g.5a5221d57.
2024-05-09 12:00:20 provisioningserver.utils.services: [warn] Couldn't report test results: HTTP error [500]
```

Even though there are multiple open bug reports that deal with the `twisted.internet.error.MulticastJoinError` I believe it is not the source of the issues here.

The true problem is the line `Couldn't report test results: HTTP error [500]`.

The complementary part on regiond from `/var/snap/maas/common/log/regiond.log`:
```
2024-05-09 12:01:20 maasserver: [error] ################################ Exception: Status for scriptresult 5462 is not running or pending (2) ################################
2024-05-09 12:01:20 maasserver: [error] Traceback (most recent call last):
  File "/snap/maas/35359/usr/lib/python3/dist-packages/django/core/handlers/base.py", line 181, in _get_response
    response = wrapped_callback(request, *callback_args, **callback_kwargs)
  File "/snap/maas/35359/lib/python3.10/site-packages/maasserver/utils/views.py", line 298, in view_atomic_with_post_commit_savepoint
    return view_atomic(*args, **kwargs)
  File "/usr/lib/python3.10/contextlib.py", line 79, in inner
    return func(*args, **kwds)
  File "/snap/maas/35359/lib/python3.10/site-packages/maasserver/api/support.py", line 62, in __call__
    response = super().__call__(request, *args, **kwargs)
  File "/snap/maas/35359/usr/lib/python3/dist-packages/django/views/decorators/vary.py", line 20, in inner_func
    response = func(*args, **kwargs)
  File "/snap/maas/35359/usr/lib/python3.10/dist-packages/piston3/resource.py", line 197, in __call__
    result = self.error_handler(e, request, meth, em_format)
  File "/snap/maas/35359/usr/lib/python3.10/dist-packages/piston3/resource.py", line 195, in __call__
    result = meth(request, *args, **kwargs)
  File "/snap/maas/35359/lib/python3.10/site-packages/maasserver/api/support.py", line 371, in dispatch
    return function(self, request, *args, **kwargs)
  File "/snap/maas/35359/lib/python3.10/site-packages/metadataserver/api.py", line 858, in signal
    target_status = process(node, request, status)
  File "/snap/maas/35359/lib/python3.10/site-packages/metadataserver/api.py", line 680, in _process_commissioning
    self._store_results(
  File "/snap/maas/35359/lib/python3.10/site-packages/metadataserver/api.py", line 563, in _store_results
    script_result.store_result(
  File "/snap/maas/35359/lib/python3.10/site-packages/maasserver/models/scriptresult.py", line 270, in store_result
    self.status in SCRIPT_STATUS_RUNNING_OR_PENDING
AssertionError: Status for scriptresult 5462 is not running or pending (2)
```

## Fix

I tracked the error to [this line](https://github.com/canonical/maas/blob/cb2b108305c2dbd217853e8c5476c30f56d22081/src/maasserver/models/scriptresult.py#L269). But this `assert` should have never been reached because it is in the block started by:
```
    if self.script_set.node.is_commissioning():
```

So the `status` of the Node object in DB for the rackd has wrong status. And indeed:

```
$ sudo snap run --shell maas -c 'maas-region shell'
>>> from maasserver.models import *
>>> Node.objects.get(hostname="sf-maasregion-1").status
>>> 0
```

`0 =` New, so I am setting `6` for Deployed.

```
>>> n=Node.objects.get(hostname="sf-maasregion-1")
>>> n.status = 6
>>> n.save()
Ctrl-D

$ sudo systemctl restart snap.maas.supervisor.service
```

And that did the trick.
