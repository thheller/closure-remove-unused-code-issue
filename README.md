


Download necessary things

```bash
curl -O https://unpkg.com/quill@1.3.5/dist/quill.js
curl -O http://repo1.maven.org/maven2/com/google/javascript/closure-compiler/v20180204/closure-compiler-v20180204.jar
curl -O http://repo1.maven.org/maven2/com/google/javascript/closure-compiler/v20180101/closure-compiler-v20180101.jar
```

Compile with bad version

```
java -jar closure-compiler-v20180204.jar --js quill.js -O SIMPLE --js_output_file compiled.js
```

Open Browser

```
open public/index.html
```

Fails with Error (See Browser Console)

```
Uncaught TypeError: Cannot call a class as a function
    at e.a (compiled.js:102)
    at new e (compiled.js:229)
    at a.value (compiled.js:218)
    at new c (compiled.js:76)
    at index.html:4
```

Compile with good version

```
java -jar closure-compiler-v20180101.jar --js quill.js -O SIMPLE --js_output_file compiled.js
```

Reload browser and Error is gone.

Running `git bisect` traced down this issue to this commit:

```
6b807c063d42a463e3c32e5911c695a0976e2b67 is the first bad commit
commit 6b807c063d42a463e3c32e5911c695a0976e2b67
Author: bradfordcsmith <bradfordcsmith@google.com>
Date:   Wed Jan 17 14:24:56 2018 -0800

    RemoveUnusedCode: remove `x instanceof UnusedName`

    -------------
    Created by MOE: https://github.com/google/moe
    MOE_MIGRATED_REVID=182269894

:040000 040000 d44ac3cdd98601b33b21192fada4fc0957a18efc ac0a44df4d8bee7350d32e3bbe4933a15f9d5151 M      src
:040000 040000 a6510a4aa77bde61508ea6c33fbdf6601f8b61e0 feb62fb9bd9b4cd4cea6f3b32ee552b293971c70 M      test
```