---
layout: post
title: Analog時計01
date:   2021-09-04
categories: ClojureScript ブログ
---

# ソースファイルから不要な行を削除する

結果のdiff
```
% git diff
diff --git a/analog-clock/src/my_cljs_projects/analog_clock.cljs b/analog-clock/src/my_cljs_projects/analog_clock.cljs
index 799d63d..e01817b 100644
--- a/analog-clock/src/my_cljs_projects/analog_clock.cljs
+++ b/analog-clock/src/my_cljs_projects/analog_clock.cljs
@@ -6,8 +6,6 @@
 
 (println "This text is printed from src/my_cljs_projects/analog_clock.cljs. Go ahead and edit it and see reloading in action.")
 
-(defn multiply [a b] (* a b))
-
 ;; define your app data so that it doesn't get over-written on reload
 (defonce app-state (atom {:text "Hello world!"}))
 
diff --git a/analog-clock/test/my_cljs_projects/analog_clock_test.cljs b/analog-clock/test/my_cljs_projects/analog_clock_test.cljs
index dcf057f..8dbb5a3 100644
--- a/analog-clock/test/my_cljs_projects/analog_clock_test.cljs
+++ b/analog-clock/test/my_cljs_projects/analog_clock_test.cljs
@@ -1,10 +1,4 @@
 (ns my-cljs-projects.analog-clock-test
     (:require
-     [cljs.test :refer-macros [deftest is testing]]
-     [my-cljs-projects.analog-clock :refer [multiply]]))
+     [cljs.test :refer-macros [deftest is testing]]))
 
-(deftest multiply-test
-  (is (= (* 1 2) (multiply 1 2))))
-
-(deftest multiply-test-2
-  (is (= (* 75 10) (multiply 10 75))))
(END)
```