# 正则表达式

## 1.1 正则表达式概述

    正则表达式的概念：
        正则表达式(英文为regular Expression)是一种[字符串检索模式]
        正则表达式具体表现为一个字符串的样子。
    正则表达式实行原理是：
        通过[参数字符串]设置检索规则，在[指定字符串]中检索符合规则的字符串。
    正则表达式的作用是：
        可以用来进行文本搜索和文本替换。

## 1.2 正则表达式的基本语法

    语法：/正则表达式主体/修饰符(可选)
    例如：var frk_reg = /beixi/i;
    其中
    (1)/beixi/i是一个正则表达式
    (2)beixi是这个正则表达式的主体，表示想要检索的内容是frank
    (3)i(ignore)是一个正则表达式的修饰符，表示检索内容时不区分大小写
    (4)正则表达式本质上是一个对象类型，只是长得像字符串

## 1.3 正则表达式常见用法

    正则表达式在实际开发中一般不会单独使用，而是会配合一些方法来完成某种功能。因为正则表达式的作用是对字符串进行操作，所有一般在实际开发中正则表达式会配合字符串的search和replace方法来使用。
    (1)search方法：用于检索与正则表达式相匹配的字符串，并返回子字符串的起始位置。
        例如：在指定字符串中，通过正则表达式搜索目标子字符串。并且不区分大小写。
            var str = 'hello World! goodbye world!'
            var index = str.search(/world/i)
            console.log(index)
        代码执行结果为：6。
        注意:a.如果能找到，返回的是第一次出现的下标
            b.如果找不到，返回-1
    (2)replace方法：用于在指定字符串中用一个字符串代替一个与正则表达式相匹配的子字符串。
        例如：在指定字符串中，通过正则表达式替换指定字符串中的目标字符串
            var str = 'hello world! goodbye world!'
            var newStr = str.replace(/world/gi,'sxt')
            console.log(newStr)
        代码执行结果为：hello sxt! goodbye sxt!
        显然replace方法的作用是替换第一个匹配到的字符串，所以我们仅替换第一个符合规则的world。而这种功能是不能满足我们的，因为原本我们不使用正则replace方法就能实现这样的功能，那么能否有一种办法能够让我们一次性修改所有符合规则的world呢？
        答案是肯定的，如在修饰符中使用加入g(global)表示全局，当没有g时只会替换第一个与正则表达式相匹配的子字符串，如果加上g会替换所有与正则表达式相匹配的子字符串
        注意：a.replace方法本身不会改变原字符串，而是会生成新的字符串。
    (2)match方法：能够匹配符合参数规则的字符串第一次出现的信息。
        例如：在指定字符串中，通过正则表达式返回目标子字符串在指定字符串中第一次出现的信息(不加g时)
            var str = 'hello world! goodbye world!'
            var info = str.match(/world/i)
            console.log(info)
        代码执行结果为：["world", index: 6, input: "hello world! goodbye world!", groups: undefined]
        当在修饰符中使用g，则可以匹配所有符合参数规则的子字符串，并构成数组返回
            var str = 'hello world! goodbye world!'
            var info = str.match(/world/Gi)
            console.log(info)
        代码执行结果为：["world", "world"]

# 2.正则表达式进阶

## 2.1 修饰符

    修饰符是正则表达式进行字符串检索时[检索规则]的制定者之一。
    修饰符规定了正则应按照何种方式进行检索。
    常见的修饰符类型有三种：i、g、m
    (1)i(ignore)修饰符，表示正则检索内容时不区分大小写
        例如：使用i修饰符，在str中检索world第一次出现的下标
            var str = 'hello World! goodbye World!'
            var index = str.search(/world/i)
            console.log(index)
        代码执行结果为：6
        不使用i修饰符，在str中检索world第一次出现的下标
            var str = 'hello World! goodbye World!'
            var index = str.search(/world/)
            console.log(index)
        代码执行结果为：-1
    (2)g(global)修饰符，表示正则检索内容时采用全局匹配，而不是找到第一个就停止。
        例如：使用g修饰符，在str中替换所有的world为sxt
            var str = 'hello world! goodbye world!'
            var newStr = str.replace(/world/gi,'sxt')
            console.log(newStr)
        代码执行结果为：hello sxt! goodbye sxt!
        不使用g修饰符，在str中替换所有的world为sxt
            var str = 'hello world! goodbye world!'
            var newStr = str.replace(/world/i,'sxt')
            console.log(newStr)
        代码执行结果为：hello sxt! goodbye world!
    (3)m(more)修饰符，表示匹配换行符两端的潜在匹配，对正则中的^$符合会有影响(不常用)

## 2.2 检索模式

    正则表达式的检索模式，用于指定正则采用何种方式进行内容检索(即正则表达式的主体可以采用何种发生编写，也是规则的制定者之一)。
    常见的检索模式有：表达式模式、元字符模式和量词模式三种。
    他们并不互相独立而是相辅相成的关系，就像修饰符可以多个一起使用一样。
    注意：a.检索模式中的几种模式可以混合使用，来达到精确指定规则的效果。
          b.在正则主体中一个[]代表一个字符，一个()代表一个词组。
    (1)表达式模式
        正则表达式的书写方式是通过表达式编写的模式称为表达式模式(即通过表达式书写来指定检索规则)。
        常见的表达式模式有一下三种：
            a)[abc]
            b)[0-9]
            c)(m|n)
        每一种模式中的内容都是表示一类值，而不是字面的含义。
        a.[abc]：在指定字符串中检索，查找任何满足条件[存在于中括号中]规则的字符或字符串(满足[]中任意字符的即符合)。
            例如：在str中替换所有：只要满足[是a、b当中之一]的字符为(frank)
                var str = '12abc12ABC'
                var newStr = str.replace(/[ab]/gi,'(frank)')
                console.log(newStr)
            代码执行结果为：12(frank)(frank)c12(frank)(frank)C
            注意：本模式针对正则主体，因此中括号必须写在主体中才会生效。
            正则不只是能替换英文，对于中文正则仍然生效
                var str = '你好，弗兰克！ 再见，弗兰克！'
                var newStr = str.replace(/[弗兰克]/gi,'(frank)')
                console.log(newStr)
            代码执行结果为：你好，(frank)(frank)(frank)！ 再见，(frank)(frank)(frank)！
            正则匹配字符串是采用多个方括号即可
                var str = '你好，弗兰克！ 再见，弗兰克！'
                var newStr = str.replace(/[弗][兰][克]/gi,'(frank)')
                console.log(newStr)
            代码执行结果为：你好，(frank)！ 再见，(frank)！
            两种方式组合，全局忽略大小写，检索两个挨在一起的字符，规则：第一个必须是1/2，第二个必须是a/b。
                var str = '12abc12ABC'
                var newStr = str.replace(/[12][ab]/gi,'(sxt)')
                console.log(newStr)
            代码执行结果为：1(sxt)bc1(sxt)BC
        b.[0-9]：在指定字符串中检索，查找任何满足[0至9之间]规则的字符或字符串(存在于范围内即满足)。该模式对字母也适用。
            例如：在str中替换所有：[任意是0-9之间之一]的字符串为(frank)
                var str = '12abc12ABC'
                var newStr = str.replace(/[0-9]/gi,'(frank)')
                console.log(newStr)
            代码执行结果为：(frank)(frank)abc(frank)(frank)ABC
            在str中替换所有：[任意是a-z之间之一]的字符串为(frank)
                var str = '12abc12ABC'
                var newStr = str.replace(/[a-z]/gi,'(frank)')
                console.log(newStr)
            代码执行结果为：12(frank)(frank)(frank)12(frank)(frank)(frank)
            在str中替换所有：[任意是a-z或A-Z之间之一]的字符串为(frank)
                var str = '12abc12ABC'
                var newStr = str.replace(/[a-zA-Z]/g,'(frank)')
                console.log(newStr)
            代码执行结果为：12(frank)(frank)(frank)12(frank)(frank)(frank)
            在str中替换所有：[任意是a-z或A-Z或0-9之间之一]的字符串为(frank)
                var str = '12abc12ABC'
                var newStr = str.replace(/[a-zA-Z0-9]/g,'(frank)')
                console.log(newStr)
            代码执行结果为：(frank)(frank)(frank)(frank)(frank)(frank)(frank)(frank)(frank)(frank)
            在str中替换所有：[任意是a-z或A-Z之间之一，并且是三个字符组合]的字符串为(frank)
                var str = '12abc12ABC'
                var newStr = str.replace(/[a-zA-Z][a-zA-Z][a-zA-Z]/g,'(frank)')
                console.log(newStr)
            代码执行结果为：12(frank)12(frank)
            注意：在表达式模式中，检索格式不能写成[9-0]，这种写法是违法的([from-to]from必须小于to)。
        c.(m|n):在指定字符串中检索，查找任何满足(以|分割的选项之一)的字符或字符串。
            例如：在str中替换所有(ab|ABC)的字符串为(frank)
                var str = '12abc12ABC'
                var newStr = str.replace(/(ab|ABC)/g,'(frank)')
                console.log(newStr)
            代码执行结果为：12(frank)c12(frank)
            注意：本模式不仅可以匹配字符，还可以匹配辞职。
    (2)元字符模式
        元字符：具有特殊含义的字符称为元字符。
        通过元字符来进行正则检索的模式，称为元字符模式。
        常见的元字符模式有一下三种：
            a)\d
            b)\s
            c)\b
        此外还有\w,\W...
        一个元字符代表一个字符。
        \d：在指定字符串中检索，查找任何[是数字]规则的字符或字符串(所有数字都匹配)。
            例如：在str中检索将数字替换为(frank)
                var str = '12abc12abABC'
                var newStr = str.replace(/\d/g,'(frank)')
                console.log(newStr)
            代码执行结果为：(frank)(frank)abc(frank)(frank)abABC
            元字符也可以组合使用，在str中检索将两个连续数字替换为(frank)
                var str = '12abc12abABC'
                var newStr = str.replace(/\d\d/g,'(frank)')
                console.log(newStr)
            代码执行结果为：(frank)abc(frank)abABC
        \s：在指定字符串中检索，查找任何[是空白]规则的字符或字符串。
            例如：在str中检索将空格替换为(frank)
                var str = ' 12abc 12ab ABC'
                var newStr = str.replace(/\s/g,'(frank)')
                console.log(newStr)
            代码执行结果为：(frank)12abc(frank)12ab(frank)ABC
        \b：在指定字符串中检索，查找任何[是单词边界]规则的字符串后字符串(单词即指多个连续字符，单词边界是看不见的，就在单词的作用两侧)
            例如：在str中检索将单词边界替换为(frank)
                var str = '12abc 12ab ABC'
                var newStr = str.replace(/\b/g,'(frank)')
                console.log(newStr)
            代码执行结果为：(frank)12abc(frank) (frank)12ab(frank) (frank)ABC(frank)
            连续的\b没有作用，但是可在连续\b中间写入字符或字符串，可以用来匹配\b内的单词
            在str中检索12ab替换为(frank)
                var str = '12abc 12ab ABC'
                var newStr = str.replace(/\b12ab\b/g,'(frank)')
                console.log(newStr)
            代码运行结果为：12abc (frank) ABC   (此时仅12ab这个单词背替换了，12abc中的12ab没变)
        \w：在指定字符串中检索，查找任何[a-zA-Z0-9_]规则的字符或字符串。
    (3)量词模式
        量词：表示要检索的字符或字符串出现的次数的词组称为量词。
        通过设置量词进行内容检索的模式称为量词模式。
        如果用n表示要检索的字符或字符串，那么常见的量词模式有以下三种：
            a)n+
            b)n*
            c)n?
        注意：a.量词通常不能单独存在于主体中，而是配合其他模式使用
              b.n除了是具体的字符或字符串外，还可以是表达式或者元字符
              c.量词是对紧挨着的字符起作用
        n+：在原字符串中检索任何[包含一个或多个n]的子字符串。匹配时实际是，能匹配多少就匹配多少，直到不满足才停止。
            例如：
                var str = 'a1abb2ab3baab'
                var newStr = str.match(/a/g)
                console.log(newStr);//['a','a','a'，'a'，'a']

                var str = 'a1abb2ab3baab'
                var newStr = str.match(/ab/g)
                console.log(newStr);//['ab','ab','ab']

                var str = 'a1abb2ab3baab'
                var newStr = str.match(/a+/g)
                console.log(newStr);//['a','a','a'，'aa']

                var str = 'a1abb2ab3baab'
                var newStr = str.match(/ab+/g)
                console.log(newStr);//['abb','ab','ab']
        n*：在原字符串中检索任何[包含0个或多个n]的子字符串。
            例如：
                var str4 = 'a1abb2ab3baab'
                var newStr4 = str4.match(/a*/g)
                console.log(newStr4);//["a", "", "a", "", "", "", "a", "", "", "", "aa", "", ""]
                (最后的空字符串是多出来的，是因为懒得模式造成的)

                var str5 = 'a1abb2ab3baab'
                var newStr5 = str5.match(/ab*/g)
                console.log(newStr5);//["a", "abb", "ab", "a", "ab"]
            ps：为什么匹配单个字符的时候结果中有空字符串，而匹配多个字符构成的字符串时结果中就没有空字符串？
        n?：在原字符串中检索任何包含0个或1个n的子字符串。匹配时实际是，一满足就离开停止。
            例如：
                var str6 = 'a1abb2ab3baab'
                var newStr6 = str6.match(/a?/g)
                console.log(newStr6);//["a", "", "a", "", "", "", "a", "", "", "", "a", "a", "", ""]

                var str7 = 'a1abb2ab3baab'
                var newStr7 = str7.match(/ab?/g)
                console.log(newStr7);//["a", "ab", "ab", "a", "ab"]

## 2.3RegExp 对象

        上面我们提到过正则表达式的基本语法是：/正则表达式主体/修饰符(可选)，这种创建正则的方法我们称之为[字面量创建正则表
    达式]。
        而实际上在js中已经为正则表达式提供了一个构造函数RegExp，我们可以通过这个构造函数来生成一个正则表达式的实例，而这种
    创建正则的方法我们称之为[构造函数创建正则表达式]。
        对于通过RegExp构造函数生成的正则实例来说，我们即能够和之前使用正则表达式一样将这个正则实例直接使用，还能够通过这个
    实例来调用一些RegExp所独有的方法。
    语法：
        var regExp = new RegExp(正则表达式内容，修饰符)；
        ps：通过构造函数方式创建正则，参数均采用字符串来声明。
    例如：
        var regExp = new RegExp('ab','g')
        var str = '12abc12abABC'
        var newStr = str.replace(regExp,'(frank)')
        console.log(newStr)
    代码执行结果：12(frank)c12(frank)ABC
    在使用构造函数创建正则表达式的时候，在其中使用元字符时，注意\表示转译符，要正常使用元字符需要些两个\\。
    例如：
        var regExp1 = new RegExp('\\d[a-z]+','gi')
        var str1 = '123abc12abABC'
        var newStr1 = str1.replace(regExp1,'(frank)')
        console.log(newStr1)
    代码执行结果：12(frank)1(frank)
    (1)RegExp对象的test()方法
        本方法是RegExp对象中提供的一个方法，用来判断在[指定字符串中]是否存在[满足正则表达式规则]的子字符串。存在就返回true，反之返回false。
        语法：
            正则表达式.test(指定字符串)
        例如：
            var regExp = new RegExp('abcd','g')
            console.log(regExp.test('12abc12abABC'));
        代码执行结果为：false
            var regExp2 = new RegExp('abc','g')
            console.log(regExp2.test('12abc12abABC'));
        代码执行结果为：true
        注意：其实RegExp中提供的方法对于字面量创建还是函数创建正则表达式都是通用的，也就意味着下面的代码也是合法的：
            console.log(/abc/g.text('12abc12abABC));    //true
    (2)RegExp对象的exec()方法
        本方法是RegExp对象中提供的一个方法，用来查找在[指定字符串中][第一个][满足正则表达式规则]的子字符串出现的[下标和内容。
        本方法的返回值是一个信息集合(对象)，但是可以当做数组一样使用。若查找失败，则返回结果为null。
        语法：
            正则表达式.exec(指定字符串)
        例如：
            var regExp4 = new RegExp('abc','gi')
            var result = regExp4.exec('12abc12abABC')
            console.log(result)             // ["abc", index: 2, input: "12abc12abABC", groups: undefined]
            console.log(result.length)      // 1
            var result = regExp4.exec('12abc12abABC')
            console.log(result)             // ["ABC", index: 9, input: "12abc12abABC", groups: undefined]
            console.log(result.length)      //1
            var result = regExp4.exec('12abc12abABC')
            console.log(result)             // null
        小括号()表示主匹配符合，若exec()匹配的正则中存在主匹配符号，会先匹配正则内容，然后在匹配一次主匹配内的内容，并且把两次的匹配结果放在一个集合中返回去。
        例如：
            var regExp5 = new RegExp('(ab)c','gi')
            var result1 = regExp5.exec('12abc12abABC')
            console.log(result1);           // ["abc", "ab", index: 2, input: "12abc12abABC", groups: undefined]
            console.log(result1.length);    // 2
            var result1 = regExp5.exec('12abc12abABC')
            console.log(result1)            // ["ABC", "AB", index: 9, input: "12abc12abABC", groups: undefined]
            console.log(result1.length);    // 2
            var result1 = regExp5.exec('12abc12abABC')
            console.log(result1)            // null
        正则对象中实际存在一个隐式的参数lastIndex，本参数指代上一次匹配结束时的下标。正常情况exec()每一次匹配都是从上一次匹配到的字符或字符串的最后一个字符的下标的下一位开始匹配，例如上面第一次匹配到234位，则第二次匹配会从5开始，即已经匹配过的部分不会再次匹配，但是获取到下标都是相对于整个目标字符串的下标。但如果设置了lastIndex,则可以让此次匹配从任意位置开始。
        例如：
            var regExp6 = new RegExp('abc','gi')
            var result2 = regExp6.exec('12abc12abABC')
            console.log(result2)            // ["abc", index: 2, input: "12abc12abABC", groups: undefined]
            console.log(result2.length)     // 1
            // 修改上一次匹配结束是保存的下标，下一次匹配从整个索引开始
            regExp6.lastIndex=0
            var result2 = regExp6.exec('12abc12abABC')
            console.log(result2)            // ["abc", index: 2, input: "12abc12abABC", groups: undefined]
            console.log(result2.length);    // 1
            var result2 = regExp6.exec('12abc12abABC')
            console.log(result2)            // ["ABC", index: 9, input: "12abc12abABC", groups: undefined]
            console.log(result2.length);    // 1
            var result2 = regExp6.exec('12abc12abABC')
            console.log(result2)            // null
        此时第二次匹配的结果和第一次一样。
        注意：a.需要注意的是，尽管我们能够像使用数组一样来使用exec()方法的返回结果，但那只是针对于查找的子字符串而言，对于子字符串出现的下标位置的访问仍需要按照属性访问来执行。即result.index或result['index]
              b.exec方法返回的结果的length其实就是表示匹配了几次，一般情况下是1，即匹配了一次，当使用主匹配符合()时，则可以返回2。
        