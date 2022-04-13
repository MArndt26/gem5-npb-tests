## Shell Startup

- Remember to activate the python virtual env

```bash
source venv/bin/activate
```

## Notes

- Broken link for run script can be found for [run_npb.py](https://github.com/darchr/gem5art-experiments/blob/master/gem5-configs/configs-npb-tests/run_npb.py) and [system/](https://github.com/darchr/gem5art-experiments/tree/master/gem5-configs/configs-npb-tests/system)

## DB Setup

```bash
sudo setfacl --modify user:gem5project:rw /var/run/docker.sock

docker run -p 27017:27017 -v /home/gem5project/npb-tests/docker:/data/db --name mongo-npb -d mongo

docker run -p 27017:27017 -v /home/gem5project/npb-tests/docker_mongo4:/data/db --name mongo4-npb -d mongo:4.4.13-focal

sudo apt-get install rabbitmq-server
```

### Start Celery Server

```bash
celery -A gem5art.tasks.celery worker --autoscale=4,0 -E
```

## Changes

- celery command ordering needed to be changed from tutorial to that given above
- [GridFS Breaking Change](https://pymongo.readthedocs.io/en/stable/migrate-to-pymongo4.html#disable-md5-parameter-is-removed)
  - File "/home/gem5project/npb-tests/venv/lib/python3.8/site-packages/gem5art/artifact/\_artifactdb.py", line 133, in **init**
    - prev: `self.fs = gridfs.GridFSBucket(self.db, disable_md5=True)`
    - new: `self.fs = gridfs.GridFSBucket(self.db)`
- Rebuilt gem5 with `scons build/X86_MESI_Two_Level/gem5.opt --default=X86 PROTOCOL=MESI_Two_Level SLICC_HTML=True -j8`
- made changes to gem5art librarys in venv
  - search `FIXME:` of `MARNDT26`

## Resources

- [gem5 official tutorial](https://www.gem5.org/documentation/gem5art/tutorials/npb-tutorial)
- [github tutorial with common fixes](https://github.com/gem5/gem5-resources/blob/stable/src/x86-ubuntu/README.md)
