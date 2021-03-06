# Итерационные методы

Термин "итерационный метод" подразумевает, что процесс вычислений заключается
в многократном вычислении (итерировании) некоторой функции на
результате предыдущего применения этой функций.
Так как функций для итерации бесконечно много, то итерационных
методов также бесконечное число.
Мы рассмотрим несколько простых методов, которые имеют самостоятельное
значения, и позволят нам развить интуитивное понимание того, как строить
новые итерационные схемы.

Рассмотрим сначала задачу нахождения суммы геометрической прогрессии:

$$S=\sum_{n=0}^\infty q^n.$$

Ответ нам известен, сумма равна

$$S=(1-q)^{-1}.$$

Чтобы воспользоваться аналитической формулой, мы должны посчитать обратное
к $1-q$, что может быть не так просто, например, если $q$ матрица.
Покажем, как найти сумму, не вычисляя обратного.

Рассмотрим частичную сумму
$$S_N=\sum_{n=0}^N q^n.$$
По определению суммы ряда, $S$ является пределом частичных сумм $S_N$ при $N\to\infty$.
По определению предела последовательности частичные суммы $S_N$ отличаются от 
предельного значения $S$ не больше, чем на сколь угодно малое значение,
начиная с некоторого числа слагаемых.
Поэтому для приближенного вычисления $S$ достаточно вычислить $S_N$
для достаточно большого $N$.
Вычислять частичные суммы можно по рекуррентной формуле, последовательно добавляя слагаемые:

$$S_{N}=\sum_{n=0}^Nq^n=(\sum_{n=0}^{N-1}q^n)+q^N=S_{N-1}+q^N.$$

Хотя такой подход естественен для человека,
найденные по этой формуле значения будут иметь
низкую точность,
так как погрешность арифметических операций будет накапливаться при росте числа слагаемых $N$.
Более того, последовательное суммирование почти никогда
не позволяет получить ответ с близкой к машинной точностью.
Действительно, погрешность численного суммирования складывается 
из погрешности отбрасывания остатка ряда и из погрешностей операций с плавающей запятой.
Вторая погрешность стартует с машинной и увеличивается с ростом числа слагаемых.
Погрешность отбрасывания остатка напротив уменьшается с увеличением числа слагаемых.
Результирующая погрешность достигает минимума при некотором конечном числе слагаемых
и не может быть меньше каждой из вышеуказанных погрешностей.

Искомую сумму все же оказывается можно найти с высокой точностью,
но для этого нужно изменить подход к суммированию.
Рассмотрим другую рекуррентную формулу для тех же частичных сумм:

$$S_{N}=\sum_{n=0}^Nq^n=1+\sum_{n=1}^{N}q^n=1+qS_{N-1}=f(S_{N-1}).$$

Новая формула также выражает $S_N$ через $S_{N-1}$,
но эта связь не содержит явно номера $N$,
что допускает новую интерпретацию алгоритма.
Пусть у нас есть некоторое приближение $S_{N-1}$ для суммы $S$,
тогда формула позволяет вычислить более точную оценку $S_N$ для $S$.
Такое прочтение формулы делает понятным, 
что погрешности операций с плавающей запятой не накапливаются, 
так как корректируются на каждом шаге.

В приведенном примере мы сохранили точное значение каждого члена
последовательности $S_N$, изменив только метод их вычисления.
Однако изменения могли бы быть и более радикальными.
Уточним, какими свойствами должны обладать $S_N$, чтобы дать в пределе $S$.
Чтобы найти $S_N$, мы должны применить $N$ раз функцию $f$ к $S_0=1$, 
т.е. мы получили итерационную формулу $S_N=f(S_{N-1})$.
Так как и $S_N$ и $S_{N-1}$ стремятся к $S$, а $f$ непрерывная функция,
то в пределе $N\to\infty$ справедливо равенство:

$$S=f(S),$$

откуда следует $S=1+qS$, т.е. $S=1/(1-q)$ и мы действительно получили сумму ряда.
Значение $S$, такое что $S=f(S)$, 
называют неподвижной точкой функции $f$.
Мы видим, что итерационный метод позволяет 
находить неподвижные точки функций,
т.е. чтобы составить итерационный метод, мы должны 
представить искомое значение в виде неподвижной точки
какой-либо функции, а затем итерировать эту функцию.

Использование ряда подсказало нам итерационную формулу,
однако можно составить множество других формул не используя ряд.
Начнем с тождества, которое мы хотим получить в пределе:
$S=1/(1-q)$, или эквивалентно $0=1-(1-q)S$.
Умножим левую и правую часть на константу $\alpha$, 
и прибавим к левой и правой части $S$:

$$S=S+\alpha(1-(1-q)S).$$

Заменим теперь $S$ слева на $S_N$, а справа на $S_{N-1}$,
получая новую итерационную формулу:

$$S_N=S_{N-1}+\alpha(1-(1-q)S_{N-1}),$$

Если предел $S_N$, определенных этими итерациями существует,
то переходя к пределу $N\to\infty$, мы получаем в пределе
вышеприведенное уравнение на $S$, которое имеет единственное решение
$S=1/(1-q)$.
Таким образом мы сконструировали новую итерационную формулу 
для решения прежней задачи.

Построенный нами выше метод нахождения обратного 
не слишком практичен для обращения чисел, однако он
действительно используется для решения линейных систем и 
называется [методом Якоби](https://ru.wikipedia.org/wiki/%D0%9C%D0%B5%D1%82%D0%BE%D0%B4_%D0%AF%D0%BA%D0%BE%D0%B1%D0%B8).
Метод Якобы не самый эффективный метод решения систем,
позже мы изучим более быстрые методы.

Будет ли результат итерирования по новой формуле сходится к $S$?
И если будет, то будет ли он сходится быстрее или медленнее формулы
для суммы геометрической прогрессии?
Ответ на этот вопрос дает 
[теорема Банаха о неподвижной точке](ru.wikipedia.org/wiki/Теорема_Банаха_о_неподвижной_точке).
Эта теорема работает со [сжимающими отображениями](https://ru.wikipedia.org/wiki/%D0%A1%D0%B6%D0%B8%D0%BC%D0%B0%D1%8E%D1%89%D0%B5%D0%B5_%D0%BE%D1%82%D0%BE%D0%B1%D1%80%D0%B0%D0%B6%D0%B5%D0%BD%D0%B8%D0%B5),
т.е. такими функциями, что они сближают точки.
Строго говоря, функция $f$ задает сжимающее отображение, если найдется константа $C<1$,
такая что

$$\mathrm{dist}(f(x),f(y))\leq C\cdot\mathrm{dist}(x,y),$$

здесь $\mathrm{dist}(x,y)$ обозначает расстояние между
точками $x$ и $y$.
Расстояние между числами определено естественным образом:

$$\mathrm{dist}(x,y)=|x-y|.$$

Наименьшая из констант $C$ называется коэффициентом сжатия.
Теорема о неподвижной точке утверждает, что если отображение
$f$ сжимающее, то последовательность итераций 
$x_n=f(x_{n-1})$ сходится к некоторой точке $x^*$,
и эта точка должна быть 
[неподвижной точкой](https://ru.wikipedia.org/wiki/%D0%9D%D0%B5%D0%BF%D0%BE%D0%B4%D0%B2%D0%B8%D0%B6%D0%BD%D0%B0%D1%8F_%D1%82%D0%BE%D1%87%D0%BA%D0%B0) для $f$, т.е.

$$x^*=f(x^*).$$

Таким образом, согласно тоеореме Банаха, для того чтобы
доказать сходимость итераций, достаточно показать,
что формула итерирования отвечает сжимающему отображению.
Рассмотрим итерации с релаксацией, имеющие вид:

$$S_n=f(S_{n-1}),\quad f(x)=x+\alpha(1-(1-q)x).$$

Найдем рассмояние между образами произвольных точек $x$ и $y$:

$$\mathrm{dist}(f(x),f(y))=|f(x)-f(y)|
=|x+\alpha(1-(1-q)x)-y-\alpha(1-(1-q)y)|=
$$

$$=|(1-\alpha(1-q))(x-y)|=|1-\alpha(1-q))|\cdot|x-y|
=|1-\alpha(1-q))|\mathrm{dist}(x,y).$$

Т.е. коэффициент сжатия для $f$ равен
$C=|1-\alpha(1-q)|$.
Чтобы отображение было сжимающим, а следовательно,
чтобы итерации сходились, достаточно, чтобы

$$C=|1-\alpha(1-q)|<1.$$

Отсюда мы можем найти $\alpha$, для которых итерации 
будут сходиться:

$$-1<1-\alpha(1-q)<1,$$

$$-2<-\alpha(1-q)<0,$$

$$0<\alpha(1-q)<2,$$

$$0<\alpha<2/(1-q),$$

если $1-q>0$, и 

$$2/(1-q)<\alpha<0,$$

если $1-q<0$.

Важно отметить, что хотя частичные суммы геометрической 
прогрессии определены только для $|q|<1$,
мы смогли построить итерационный метод для
нахождения $S=1/(1-q)$ для всех значений $q$,
подбирая подходящий коэффициент релаксации $\alpha$.

Приведенные выше рассуждения работают только
в области, где функция $f$ является сжимвющим отображением.
В частности, если начальное приближение $S_0$ было слишком грубым, 
то оно может выпасть из области сходимости метода,
тогда последовательность приближений $S_N$ может
не иметь предела, в этом случае говорят,
что итерационный метод расходится.

Так как мы обычно хотим найти результат с некоторой
заданной точностью, то является важным вопрос,
какой параметр $\alpha$ позволяет найти ответ
с этой точностью за наименьшее число итераций,
т.е. вопрос ускорения вычислений.
Теорема Банаха позволяет ответить и на этот вопрос.
Согласно этой теореме, абсолютная ошибка оценки искомого
значения $S$ на каждом шаге уменьшается 
как минимум в коэффициент сжатия $C$ раз:

$$\mathrm{dist}(S,S_N)\leq C\mathrm{dist}(S,S_{N-1}).$$

Таким образом скорость сходимости будет максимальной,
а необходимое для вычисления значения $S$ с желаемой
точностью число итераций будет минимальным, 
если константа сжатия $C$ максимально близка к $0$.
Так в рассматриваемом примере параметр релаксации
равный $\alpha=1/(1-q)$ соответствует коэффициенту
сжатия $C=0$, т.е. первая же итерация дает верный ответ.
Однако это оптимальное $\alpha$ совпадает с искомым
значением, которое мы на практике, конечно, не знаем.
Однако выбор хорошего приближения решения в качестве
$\alpha$ может значительно ускорить вычисления.

Попробуйте ответить, почему хороший выбор для $\alpha$
более важен, чем хорошее начальное приближение $S_0$?

Мы знаем, что итерационный метод улучшает точность
на каждой шаге в $C$ раз, пока не будет достигнута
точность вычисления функции $f$.
Однако как нам узнать, что желаемая точность достигнута,
и дальнейшие вычисления избыточны?
Чтобы ответить на этот вопрос, нам потребуется еще одна
оценка точности решения из теоремы Банаха:

$$
\mathrm{dist}(S,S_N)\leq\frac{C}{1-C}\mathrm{dist}(S_N,S_{N-1}),
$$

т.е. абсолютная ошибка вычисления неподвижной точки
пропорциональна изменению решения на каждом шаге,
причем коэффициент пропорциональной легко вычисляется,
если известен коэффициент сжатия $C$,
На практике мы вычисляем изменение $\mathrm{dist}(S_N,S_{N-1})$ приближения на каждом шаге, 
и продолжаем итерации до тех пор,
пока это изменение не станет достаточно маленьким.

Приведенные выше оценки, вытекающие из теоремы Банаха,
описыают худшее возможное поведение метода,
хорошо составленные методы могут иметь более быструю
сходимость.

В приведенном выше примере вычисления 
обратного числа с релаксацией, скорость сходимости
тем выше, чем коэффициент релаксации $\alpha$ ближе
к искомому решению $S=1/(1-q)$.
Последовательные итерации $S_N$ как раз дают 
приближения для $S$, причем точность приближения
на каждой итерации растет.
Это наталкивает нас на мысль выбирать свое $\alpha$
на каждой итерации, и положиить $\alpha=S_{N-1}$.
Таким образом мы получаем следующую итерационную схему:

$$
X_N=f(X_{N-1})=X_{N-1}+X_{N-1}(1-(1-q)X_{N-1}).
$$

Задания для закрепления:

1. Найдите неподвижные точки функции $f$.

1. Найдите область значений $X$, для которые отображение $f$ сжимающее. Указание: воспользуйтесь производной $f$.

1. Оцените скорость сходимости итераций.