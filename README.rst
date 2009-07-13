MOCK
====
Mocks are what we are talking about here:
objects pre-programmed with expectations which form a
specification of the calls they are expected to receive.

>>> from ludibrio import Mock

>>> with Mock() as greetings:
...     greetings.excuse_me() >> 'Com licença'
...     greetings.hello('Gustavo') >> 'Ola, Gustavo'
...     greetings.see_you_soon >> 'Até logo'
...     greetings.see_you_soon >> 'Até logo, denovo'

>>> print greetings.excuse_me()
Com licença

>>> print greetings.hello('Gustavo')
Ola, Gustavo

>>> print greetings.see_you_soon
Até logo

>>> print greetings.see_you_soon
Até logo, denovo

>>> print greetings.see_you_soon
Traceback (most recent call last):
...
AssertionError: Object's mocks are not pre-programmed with expectations:see_you_soon


>>> with Mock() as greetings:
...     greetings.excuse_me() >> 'Com licença'
...     greetings.see_you_soon >> 'Até logo'

>>> print greetings.excuse_me()
Com licença

>>> print greetings.hello('Gustavo Rezende')
Traceback (most recent call last):
...
AssertionError: Object's mocks are not pre-programmed with expectations:hello


    >>> with Mock() as listing:
    ...     listing[1] >> 'um'
    ...     listing[1:4] >> ['dois', 'tres', 'quatro']
    ...     listing['quatro'] >> 4

    >>> print listing[1]
    um
    >>> print listing[1:4]
    ['dois', 'tres', 'quatro']
    >>> print listing['quatro']
    4


    >>> with Mock() as callable:
    ...     callable(two=2) >> 2

    >>> callable(two=2)
    2


    >>> with Mock() as operator:
    ...     operator * 4 >> 4
    ...     operator + 4 >> 5
    ...     operator ** 4 >> 1

    >>> operator * 4
    4
    >>> operator + 4
    5
    >>> operator ** 4
    1

    >>> with Mock() as Greetings:
    ...     greetings = Greetings(tree=3)

    >>> greetings
    <ludibrio.Mock object at ...>

    >>> with Mock() as MySQLdb:
    ...     con = MySQLdb.connect('servidor', ' usuario', 'senha')
    ...     con.select_db('banco de dados') >> None
    ...     cursor = con.cursor()
    ...     cursor.execute('ALGUM SQL') >> None
    ...     cursor.fetchall() >> [1,2,3,4,5]

    >>> con = MySQLdb.connect('servidor', ' usuario', 'senha')
    >>> con.select_db('banco de dados')
    >>> cursor = con.cursor()
    >>> cursor.execute('ALGUM SQL')
    >>> cursor.fetchall()
    [1, 2, 3, 4, 5]

Stubs
=====
Stubs provide canned answers to calls made during the test,
usually not responding at all to anything outside what's programmed in for the test.

>>> from ludibrio import Stub

>>> with Stub() as greetings:
...     greetings.excuse_me() >> 'Com licença'
...     greetings.hello('Gustavo') >> 'Ola, Gustavo'
...     greetings.see_you_soon = 'Até logo'


>>> print greetings.hello('Gustavo')
Ola, Gustavo

>>> print greetings.excuse_me()
Com licença

>>> print greetings.hello('Gustavo')
Ola, Gustavo

>>> print greetings.see_you_soon
Até logo


Dummy
=====
Dummy objects are passed around but never validated.

>>> from ludibrio import Dummy

>>> Dummy()
Dummy object
>>> Dummy(1,2.3,"4",a=5.6, b=7, c="8 ...")
Dummy object

>>> def execute_but_not_validated(x):
...     x.write("teste")
...     x.close()

>>> execute_but_not_validated(Dummy())

>>> dummy = Dummy()

>>> dummy.executAnythingAndReturnDummyObject()
Dummy object

>>> dummy.foo()
Dummy object
>>> dummy.bar
Dummy object
>>> dummy.one.two.three()
Dummy object

>>> 1 + Dummy()
Dummy object
>>> Dummy() -1
Dummy object

>>> dummy = Dummy()
>>> dummy ** 22 and dummy / dummy
Dummy object

>>> dummy * 69 or dummy // dummy == "Anything"
Dummy object

>>> list(Dummy())
[Dummy object]
>>> for i in Dummy():print i
Dummy object
>>> Dummy()[4]
Dummy object
>>> Dummy()[2:9]
Dummy object
>>> 8 in Dummy()
True


>>> dict(Dummy())
{Dummy object: Dummy object}

>>> str(Dummy())
'Dummy object'
>>> Dummy() %(1, "Gustavo Rezende", 2.8)
Dummy object

>>> int(Dummy())
1

>>> float(Dummy())
1.0

>>> bool(Dummy())
True

>>> del dummy.anything

>>> isinstance(Dummy(), str)
False
