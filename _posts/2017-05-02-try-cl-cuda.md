---
layout: post
title: ClojureとCUDA
date:   2017-05-02
categories: clojure
---

# ClojureとCUDA

昨日、cl-cudaを動かしました。ClojureでもGPU動かしてみたいと思いました。
「clojure cuda」でググると[clojurecuda](http://clojurecuda.uncomplicate.org/)がありました。

さっそく、「[Get Started!](http://clojurecuda.uncomplicate.org/articles/getting_started.html)」

まずは、Clojureのプロジェクトをleinで生成。

```
$ lein new clj-cuda
```

project.cljのdependancyにclojurecudaを追加。

```
$ grep cuda project.clj 
(defproject clj-cuda "0.1.0-SNAPSHOT"
                 [uncomplicate/clojurecuda "0.1.0"]])
```

```
$ lein repl
nREPL server started on port 56092 on host 127.0.0.1 - nrepl://127.0.0.1:56092
REPL-y 0.3.7, nREPL 0.2.12
Clojure 1.8.0
Java HotSpot(TM) 64-Bit Server VM 1.8.0_112-b16
    Docs: (doc function-name-here)
          (find-doc "part-of-name-here")
  Source: (source function-name-here)
 Javadoc: (javadoc java-object-or-class-here)
    Exit: Control+D or (exit) or (quit)
 Results: Stored in vars *1, *2, *3, an exception in *e

user=> (ns example
  #_=>   (:use [uncomplicate.clojurecuda core info nvrtc]))
nil
example=> (init)
true
example=> (map info (map device (range (device-count))))
({:async-engine-count 1, :managed-memory true, :multi-gpu-board false, :maximum-surface2d-layered-layers 2048, :maximum-texturecubemap-width 16384, :ecc-enabled false, :max-pitch 2147483647, :max-grid-dim-y 65535, :compute-mode :default, :can-map-host-memory true, :max-grid-dim-z 65535, :pci-bus-id-string "0000:01:00.0", :maximum-texture2d-mipmapped-width 16384, :texture-pitch-alignment 32, :kernel-exec-timeout true, :maximum-texture2d-linear-height 65000, :max-shared-memory-per-multiprocessor 49152, :total-mem 2147024896, :maximum-texture1d-layered-width 16384, :maximum-texturecubemap-layered-layers 2046, :maximum-texture3d-width 4096, :maximum-surface2d-layered-height 32768, :max-block-dim-z 64, :maximum-surface1d-width 65536, :maximum-surface3d-width 65536, :name "GeForce GT 750M", :maximum-texture3d-height-alternate 2048, :max-threads-per-multiprocessor 2048, :max-shared-memory-per-block 49152, :maximum-texture3d-width-alternate 2048, :compute-capability-major 3, :texture-alignment 512, :global-memory-bus-width 128, :maximum-surface2d-layered-width 65536, :memory-clock-rate 2508000, :maximum-surfacecubemap-layered-layers 2046, :maximum-surface2d-height 32768, :clock-rate 925500, :concurrent-kernels 1, :compute-capability-minor 0, :maximum-texture2d-width 65536, :max-threads-per-block 1024, :maximum-texture1d-linear-width 134217728, :integrated false, :maximum-texture2d-layered-layers 2048, :max-block-dim-x 1024, :maximum-texture1d-mipmapped-width 16384, :maximum-texture2d-mipmapped-height 16384, :local-L1-cache-supported true, :maximum-surface1d-layered-layers 2048, :pci-bus-id 1, :maximum-texture1d-layered-layers 2048, :maximum-surfacecubemap-layered-width 32768, :max-grid-dim-x 2147483647, :maximum-texture2d-height 65536, :global-L1-cache-supported false, :maximum-texture2d-linear-pitch 1048544, :maximum-texturecubemap-layered-width 16384, :multi-gpu-board-group-id 0, :pci-domain-id 0, :maximum-surface3d-depth 2048, :maximum-surface2d-width 65536, :stream-priorities-supported false, :multiprocessor-count 2, :tcc-driver false, :warp-size 32, :unified-addressing true, :maximum-texture3d-height 4096, :L2-cache-size 262144, :maximum-surfacecubemap-width 32768, :maximum-texture1d-width 65536, :maximum-surface1d-layered-width 65536, :maximum-surface3d-height 32768, :pci-device-id 0, :max-registers-per-block 65536, :max-block-dim-y 1024, :surface-alignment 512, :maximum-texture3d-depth-alternate 16384, :maximum-texture3d-depth 4096, :total-constant-memory 65536, :maximum-texture2d-linear-width 65000, :max-registers-per-multiprocessor 65536, :maximum-texture2d-layered-height 16384})
example=> (quit)

```


ここから先は、ClojureCLをつかうらしい。参考書は「OpenCL in action」らしい。