# async-profiler-hot-cold-flamegraph-example


[Hot/Cold Flame Graph](https://www.brendangregg.com/FlameGraphs/hotcoldflamegraphs.html) is an visualization which combines both on-CPU and off-CPU flame graphs.

This is an example of a Hot/Cold Flame Graph generated by async-profiler.

[![https://user-images.githubusercontent.com/38237246/139537472-c38803f4-adeb-479a-b14f-a83f8e4becd2.png](https://user-images.githubusercontent.com/38237246/139537472-c38803f4-adeb-479a-b14f-a83f8e4becd2.png)](https://tetsuzawa.github.io/async-profiler-hot-cold-flamegraph-example/demo.html)

- [demo.html](https://tetsuzawa.github.io/async-profiler-hot-cold-framegraph-example/demo.html)
- [demo_threads.html](https://tetsuzawa.github.io/async-profiler-hot-cold-flamegraph-example/demo_threads.html)




## How to generate

1. Launch containers

```shell
docker-compose up -d --build
```

2. Exec async-profiler to generate .jfr file in demo container

```shell
# external shell
curl http://localhost:9090/demo 
# (optional) use ab command instead of curl
```

```shell
docker-compose exec demo bash
/var/lib/async-profiler/profiler.sh -e wall,alloc,lock -d 60 -t -i 1ms -o jfr -f "demo.jfr" $(pgrep -nx java)
```

`-e wall` option **must** be used to sample regardless of thread state (on-CPU or off-CPU).

`-t` option should be used to avoid confusing off-CPU and on-CPU comparisons.

3. Convert .jfr to .html

```shell
java -cp converter.jar jfr2hotcoldflame demo.jfr demo.html
java -cp converter.jar jfr2hotcoldflame --threads demo.jfr demo_threads.html
```


