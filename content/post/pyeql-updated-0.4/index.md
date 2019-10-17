---
title: "Announcing pyEQL – a Python library for solution chemistry"
draft: false
date: 2016-06-26
publishDate: 2019-10-10T01:35:16.075216Z
authors: ["Ryan S. Kingsbury"]
---

Earlier this week I released [pyEQL v0.4](https://github.com/rkingsbury/pyEQL/releases/tag/v0.4.0). pyEQL is a side project of mine that aims to create a robust Python library for solution chemistry and chemical thermodynamic calculations. You can read more about the project in the [intial release announcement](http://blog.ryankingsbury.com/announcing-pyeql-a-python-library-for-solution-chemistry/).

#### Fixes
This update contains a couple of important bugfixes related to osmotic pressure and activity correction calculations for salts with one or more multivalent ions, and greatly expands the documentation of the calculation methods pyEQL employs. 

#### Features
#####Dielectric Constant Calculation
I've added a few new features, including the ability to calculate the dielectric constant of a salt solution based on [this empirical model](https://dx.doi.org/10.1016/j.fluid.2014.05.037). For example:
```
>>> s1=pyEQL.Solution([['Mg+2','1 mol/kg'],['Cl-','2 mol/kg']])
>>> s1.get_dielectric_constant()
<Quantity(58.17940826221023, 'dimensionless')>
```
It's amazing how much the presence of salt can lower the dielectric constant (pure water is 78).

##### More automatic solutions
I also added shortcuts for creating two solutions commonly encountered in wastewater treatment applications. For example, to create a solution that represents typical domestic water, all you have to do is type

```
>>> s1 = pyEQL.autogenerate('wastewater')
```
You can also generate a solution of urine this way, in addition to seawater and rainwater, which saves quite a bit of typing (and time spent looking up composition info). As you can see, the "medium strength" domestic wastewater contains quite a few things:
```
>>> s1.list_concentrations('mg/L')
Component Concentrations:

========================

OH-:	 0.0017 mg / l
Cl-:	 59.0000 mg / l
C6H12O6:	 410.0000 mg / l
H+:	 0.0001 mg / l
SO4-2:	 26.0000 mg / l
NH3:	 24.3000 mg / l
H2O:	 997003.5389 mg / l
K+:	 16.0000 mg / l
PO4-3:	 7.6000 mg / l
```

In the absence of a better assumption, the chemical oxygen demand in the wastewater is approximated by glucose. 

##### Easier automated testing

For those (like me) who are actively making changes to pyEQL code, its now easier to run automated tests. From the project directory, simply run
```
python3 setup.py test
```
Alternatively, from a Python shell:
```
>>> pyEQL.test()
..............x..................................xx...........x....................................................................................................
----------------------------------------------------------------------
Ran 163 tests in 15.537s

OK (expected failures=4)
<unittest.runner.TextTestResult run=163 errors=0 failures=0>
```

This will execute more than 150 checks of pyEQL's calculations against published data. Thanks to [hgrecco](https://github.com/hgrecco) for the advice on how to set this up.

#### Future work

Behind the scenes, I've made huge progress toward support for **reaction equilibria** within pyEQL. When finished, this means that you'll be able to enter the total ionic composition of a solution, and pyEQL will determine the *equilibrium* pH and speciation of everything in it. 

This is the headline feature I'm hoping to have ready for the next (v0.5) release. Longer-term, this will enable pyEQL to automatically generate [Pourbaix diagrams](https://en.wikipedia.org/wiki/Pourbaix_diagram) and the like.

In the mean time, please enjoy this release, and don't hesitate to [post an issue](https://github.com/rkingsbury/pyEQL/issues) or send feedback if you run into problems.