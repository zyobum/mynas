# Performance Testing with FIO

## Install FIO

To test the performance of your ZFS setup, install FIO:

```bash
sudo apt update
sudo apt install fio
```

## Example FIO Test

Run a basic FIO test to benchmark read and write performance:

```bash
fio --name=write_test --ioengine=sync --rw=write --bs=4k --numjobs=1 --size=1G --runtime=60 --group_reporting
```

### Parameters:

- `--name=write_test`: Name of the test.
- `--ioengine=sync`: Synchronous I/O engine.
- `--rw=write`: Write test.
- `--bs=64k`: Block size of 64KB.
- `--numjobs=1`: Single job.
- `--size=8G`: Total size of 8GB.
- `--runtime=60`: Run for 60 seconds.
- `--group_reporting`: Group reporting of results.

## Detailed FIO Test Script

You can create a script to run more detailed tests. Save the following content to a file named `fio_test.sh`:

```bash
#!/bin/bash

# Sequential Read and Write Test
fio --name=seq_read_write --ioengine=sync --rw=readwrite --bs=128k --numjobs=1 --size=8G --runtime=60 --group_reporting

# Random Read and Write Test
fio --name=rand_read_write --ioengine=sync --rw=randrw --bs=64k --numjobs=4 --size=8G --runtime=60 --group_reporting
```

## Run the Script

Make the script executable and run it:

```bash
chmod +x fio_test.sh
./fio_test.sh
```

## Analyze Results

FIO will output the performance metrics for each test, allowing you to analyze the read and write speeds of your ZFS setup.
```

These drafts provide a structured approach to setting up and documenting your ZFS-based home NAS using a Raspberry Pi 5 and SD cards, along with performance testing instructions using FIO. Feel free to customize the content further based on your specific needs and observations.
