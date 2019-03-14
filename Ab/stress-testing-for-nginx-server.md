```console
frank@stresstest:~$ ab -p goods_11.json -T application/json  -c 100 -n 10000 http://${hostname}/
This is ApacheBench, Version 2.3 <$Revision: 1706008 $>
Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
Licensed to The Apache Software Foundation, http://www.apache.org/

Benchmarking ${hostname} (be patient)
Completed 1000 requests
Completed 2000 requests
Completed 3000 requests
Completed 4000 requests
Completed 5000 requests
Completed 6000 requests
Completed 7000 requests
Completed 8000 requests
Completed 9000 requests
Completed 10000 requests
Finished 10000 requests


Server Software:        nginx/1.15.8
Server Hostname:        ${hostname}
Server Port:            80

Document Path:          /
Document Length:        153 bytes

Concurrency Level:      100
Time taken for tests:   2.085 seconds
Complete requests:      10000
Failed requests:        0
Non-2xx responses:      10000
Total transferred:      3180000 bytes
Total body sent:        1490000
HTML transferred:       1530000 bytes
Requests per second:    4795.82 [#/sec] (mean)
Time per request:       20.851 [ms] (mean)
Time per request:       0.209 [ms] (mean, across all concurrent requests)
Transfer rate:          1489.33 [Kbytes/sec] received
                        697.83 kb/s sent
                        2187.16 kb/s total

Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:        1    9   4.0      8      49
Processing:     1   10  11.4      8     241
Waiting:        1    9  11.4      8     241
Total:          3   19  12.1     18     262

Percentage of the requests served within a certain time (ms)
  50%     18
  66%     20
  75%     22
  80%     23
  90%     25
  95%     27
  98%     31
  99%     33
 100%    262 (longest request)
frank@stresstest:~$
```
