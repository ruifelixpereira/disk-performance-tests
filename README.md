# Disk performance tests


## References:
- https://learn.microsoft.com/en-us/azure/virtual-machines/disks-benchmarks
- https://learn.microsoft.com/en-us/azure/azure-local/manage/diskspd-overview
- https://learn.microsoft.com/en-us/azure/virtual-machines/disks-metrics



## Test design

- 120 seconds duration + 30 seconds warm-up
- 200G sample file size
- 8k block size for write tests
- 4k block size for read and mixed read/write tests
- 128 queue depth (number of outstanding I/Os per target per thread)


write: 200G, 8k, 128 depth
read: 200G, 4k, 128 depth
read/write: 200G, 8k, 128 depth
throughput: 200G, 64k, 128 depth



## Diskspd tests


### Test 1: 4K random read/write

```bash
# write
diskspd -c200G -w100 -b8K -F4 -r -o128 -W30 -d120 -Sh -D -L testfile.dat

# read
diskspd -c200G -w0 -b4K -F4 -r -o128 -W30 -d120 -Sh -D -L testfile.dat

# mixed
diskspd -c200G -w50 -b4K -F4 -r -o128 -W30 -d120 -Sh -D -L testfile.dat

# throughput
diskspd -c200G -w50 -b64K -F4 -r -o128 -W30 -d120 -Sh -D -L testfile.dat


## FIO test

sudo fio --runtime 120 --startdelay 30 fiowrite.ini
sudo fio --runtime 120 --startdelay 30 fioread.ini
sudo fio --runtime 120 --startdelay 30 fioreadwrite.ini
sudo fio --runtime 120 --startdelay 30 fiothroughput.ini

