# Snail Distances

> Points: 75

> Category: Crypto

## Description

Bob was messing around with his public key and says custom created modulus are less prone to attacks, which he thinks it's much more secure. Can you prove him wrong?

## Attachments

https://fdownl.ga/03A68DF85C

## Solution

#### Tools/Resources

1. https://www.johndcook.com/blog/2018/10/28/fermat-factoring/
2. [RsaCtfTool](https://github.com/Ganapati/RsaCtfTool)

The challenge name gives a hint that **p** and **q** must be close together.

[This article](https://www.johndcook.com/blog/2018/10/28/fermat-factoring/) does a great job in explaining why not to use similar sized primes and how to take advantage of that to find p and q.

The code below came from the article linked above

```py
from sympy import sqrt, log, ceiling, Integer

def is_square(n):
    return type(sqrt(n)) == Integer

def fermat_factor(n):
    num_digits = int(log(n, 10).evalf() + 1)
    a = ceiling( sqrt(n).evalf(num_digits) )

    counter = 0
    while not is_square(a*a - n):
        a += 1
        counter += 1

    b = sqrt(a*a - n)
    return(a+b, a-b, counter)
n = 10931382846841906873871535877090540313629164987208690260540569635114744580966366528131362680953824609267382733208234798940687863000116690164422420154297642670564972265154199962576756984827496056126695993344004105743260121582445185927903374433863590735415929278090626216488577870520231505076066111506560816042564717868589889490613655717133293544493590763695122228843919107364947943161135173395350049230506418516741719458270220538941401839789253242671398827914972773272651076340346964805489694432511028234134615604915236051005160593292729877026449980831586254355896109772745588570334199465271408469367868279971532237450684980342992371840293033285427212883001158383345612405020454741659975598086714527690520154504105865450411405231086465586296564177688603734640050156649657811746183256364723273408243088144029256510552871455757676961371153553221737433424281854733759757565197317650307051963956460476975489818954417808662738922113219328596240361694023672159223011347620131117583857693434250049667709172780665411856074212221621315034330200157991226085458013161013873744408450202861761004310237068792943422238372563783779344603519234057540461649194975828087571561097174929122409465441658754593310615047032244899
print(fermat_factor(n))
```

Now that we have **p** and **q**, we can find and decode **m**, giving us the flag.

```
p = 104553253640629983380166852730147393719766290868476096236045939162546086904884073476094419965783830121194889187168595428546431909077648045500398421061113074433828809531142344440821923011504888665305510441092717270454973945537451143462287927863373936599164398343492237210599386116749469699232918270553741225412030510746549209571625057145502221546731576211104320092532965903196764677568187256761154714839050877736942391472782979302354843683281504889403547875867523243338563070619862259498733242615357694585638343689929600615716394365010834256474062261804084868565411610839841413627472888873250195208429293

q = 104553253640629983380166852730147393719766290868476096236045939162546086904884073476094419965783830121194889187168595428546431909077648045500398421061113074433828809531142344440821923011504888665305510441092717270454973945537451143462287927863373936599164398343492237210599386116749469699232918270553741225412030510746549209571625057145502221546731576211104320092532965903196764677568187256761154714839050877736942391472782979302354843683281504889403547875867523243338563070619862259498733242615357694585638343689929600615716394365010834256474062261804084868565411610839841413627472888873250195208392143

e = 65537

c =  3933330724994707808957404465355799816006306110854974411082671986027742898866941640902533669831102370327153592881985123045483513247333695766606428557861439295417260669139166813939449153091292558344423314921423093207212436317886352646607327431926702435121008718363054027629003196267635628779350592597254417017116902267337943147141767889905452909098009548051285439992920741755209995988840210596256806093839733318285282261954289399272248904883418064164236274155247871383689411189984756588085983702099526322247134979042237613087494871042770094150921324856894651395984670016936524257343132266082131103798656973012581524185488331394262894434263028310191624505704773580221239367634705290009288140812840718280007724623604676954686757477406911373532380370239540395429855979231694606338564750228774995490559636161472305570883677809120747551648213337179258178884605152701275278060497236946144014876821398936249132854160456697530996272039540209267152922200165316488180855367689412115968754834822613862841524273039587508253626723424526092497660146210053767912914080576072876167632163358544359309566244805561606007936409211794627386420078191526132778721366082057251201451002428155615241710025256784286592957774368488863
```

Plug values into [RsaCtfTool](https://github.com/Ganapati/RsaCtfTool)

```sh
$ python3 RsaCtfTool.py \
-p 104553253640629983380166852730147393719766290868476096236045939162546086904884073476094419965783830121194889187168595428546431909077648045500398421061113074433828809531142344440821923011504888665305510441092717270454973945537451143462287927863373936599164398343492237210599386116749469699232918270553741225412030510746549209571625057145502221546731576211104320092532965903196764677568187256761154714839050877736942391472782979302354843683281504889403547875867523243338563070619862259498733242615357694585638343689929600615716394365010834256474062261804084868565411610839841413627472888873250195208429293 \
-q 104553253640629983380166852730147393719766290868476096236045939162546086904884073476094419965783830121194889187168595428546431909077648045500398421061113074433828809531142344440821923011504888665305510441092717270454973945537451143462287927863373936599164398343492237210599386116749469699232918270553741225412030510746549209571625057145502221546731576211104320092532965903196764677568187256761154714839050877736942391472782979302354843683281504889403547875867523243338563070619862259498733242615357694585638343689929600615716394365010834256474062261804084868565411610839841413627472888873250195208392143 \
-e 65537 --uncipher 3933330724994707808957404465355799816006306110854974411082671986027742898866941640902533669831102370327153592881985123045483513247333695766606428557861439295417260669139166813939449153091292558344423314921423093207212436317886352646607327431926702435121008718363054027629003196267635628779350592597254417017116902267337943147141767889905452909098009548051285439992920741755209995988840210596256806093839733318285282261954289399272248904883418064164236274155247871383689411189984756588085983702099526322247134979042237613087494871042770094150921324856894651395984670016936524257343132266082131103798656973012581524185488331394262894434263028310191624505704773580221239367634705290009288140812840718280007724623604676954686757477406911373532380370239540395429855979231694606338564750228774995490559636161472305570883677809120747551648213337179258178884605152701275278060497236946144014876821398936249132854160456697530996272039540209267152922200165316488180855367689412115968754834822613862841524273039587508253626723424526092497660146210053767912914080576072876167632163358544359309566244805561606007936409211794627386420078191526132778721366082057251201451002428155615241710025256784286592957774368488863
```

### Flag

> ictf{fAct0r1ng_W1th_F3rM@t}