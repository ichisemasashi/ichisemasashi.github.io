---
layout: post
title: Analog時計02
date:   2021-09-04
categories: ClojureScript ブログ
---

# CSSの追加

アナログ時計を作るための[参考サイト](https://note.affi-sapo-sv.com/js-canvas-analog-clock.php)から、CSSをコピーする。

```
% git diff
diff --git a/analog-clock/resources/public/css/style.css b/analog-clock/resources/public/css/style.css
index 26163d2..fc63b27 100644
--- a/analog-clock/resources/public/css/style.css
+++ b/analog-clock/resources/public/css/style.css
@@ -1,2 +1,8 @@
 /* some style */
+#app{
+       height: 400px;
+       max-height: 90vh;
+       position: relative;
+       width: 100%;
+}
```

