# estrategy

## scheme 超循環評価器 

`chicken-install`してください。

~~~~~{.scheme}
csi> (use estrategy)
csi> (driver-loop)
scheme[value]> 
~~~~~

lambda式のパラメータにオプションをつけることで  
そのパラメータの評価戦略を値呼び、名前呼び、必要呼びの3つから選べるようにする。  
オプション無しだとデフォルトの評価戦略で評価されるようになる。

~~~~~{.scheme}
(lambda ((x value) (y name) (z need))
  (+ x y z))
~~~~~

`call-by`によってデフォルトの評価戦略を変更できる

~~~~~{.scheme}
scheme[value]> (call-by need)
scheme[need]> (let ((foo (lambda (x y)
                           (+ (* x 2) x))))
                (let ((goo (lambda (x)
                             (goo x))))
                  (let ((hoo (lambda (x)
                               (display "hoo ")
                               (display x)
                               (display "\n")
                               x)))
                    (foo 20 (goo 10))
                    (foo (hoo 10) (20)))))
hoo 10
30
scheme[need]> (call-by name)
scheme[name]> (let ((foo (lambda (x y)
                           (+ (* x 2) x))))
                (let ((goo (lambda (x)
                             (goo x))))
                  (let ((hoo (lambda (x)
                               (display "hoo ")
                               (display x)
                               (display "\n")
                               x)))
                    (foo 20 (goo 10))
                    (foo (hoo 10) (20)))))
hoo 10
hoo 10
30
scheme[name]> (call-by value)
scheme[value]> (let ((foo (lambda (x y)
                           (+ (* x 2) x))))
                (let ((goo (lambda (x)
                             (goo x))))
                  (let ((hoo (lambda (x)
                               (display "hoo ")
                               (display x)
                               (display "\n")
                               x)))
                    (foo 20 (goo 10))
                    (foo (hoo 10) (20)))))
Error: Unbound variable
goo 
scheme[value]> 
~~~~~
