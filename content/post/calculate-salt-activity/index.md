---
title: "How to calculate salt activity"
draft: false
date: 2016-09-29
publishDate: 2019-10-10T01:35:16.075216Z
authors: ["Ryan S. Kingsbury"]
---

My work in membrane processes for desalination and [blue energy](http://blog.ryankingsbury.com/the-untapped-source-of-clean-energy-youve-never-heard-of/) often requires me to calculate the thermodynamic properties of  salts. The tricky part of this is determining something called the **activity** of the salt.

As long as I know the salt concentration, I just need to find the [activity coefficient](https://en.wikipedia.org/wiki/Activity_coefficient) to get the activity. There are several ways to get activity coefficients, but they all give you something called the *mean ionic activity coefficient*, which has to be combined with the salt concentration in just the right way in order to get a correct result. It's easy to do with monovalent salts like sodium chloride (NaCl), but it gets tricky when you want to consider multivalent salts (like calcium chloride, CaCl<sub>2</sub>). 

I've been unable to find a complete explanation of how to use activity coefficients that is general enough to include multivalent salts online , so I decided to write one. Much of the content below is adapted from *<a  href="https://www.amazon.com/gp/product/B004SJ3AG0/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=B004SJ3AG0&linkCode=as2&tag=rkblogamz-20&linkId=fce48f7a1bdcbafc55c890fd4b7ca71f">Aquatic Chemistry</a><img src="//ir-na.amazon-adsystem.com/e/ir?t=rkblogamz-20&l=am2&o=1&a=B004SJ3AG0" width="1" height="1" border="0" alt="" style="border:none !important; margin:0px !important;display:inline" />* by Werner Stumm and James Morgan, but I've extended their treatment to more thoroughly cover multivalent salts.

### Chemical Potential

The thermodynamic quantity underlying almost all chemical calculations is the **chemical potential**, and calculating it is the reason determining the activity is so important. The chemical potential $\mu$ of any substance is given by

$$ (1) \qquad \qquad \begin{equation} \mu = \mu^o + RT \ln a \end{equation} $$

where $\mu^o$ is the chemical potential at some standard state, $a$ is the activity or "effective concentration," $R$ is the gas constant, and $T$ is the temperature. The activity is related to the actual concentration $m$ by an activity coefficient $\gamma$:

$$ (2) \qquad \qquad \begin{equation} a = m \gamma \end{equation} (2)$$

$\gamma$ is an empirical correction factor that you can look up in a table or calculate with a model like [pyEQL](linke to announcement). It's important to use a $\gamma$ and a concentration based on the same scale. In these examples I'm going to use the molal (mol/kg) scale, and denote the concentrations with $m$. You could instead use the molar (mol/L) scale, in which case it's conventional to use $C$ for concentration.

### Application to salts in water

I'm interested in the chemical potential of salts dissolved in water, and I'll start with a monvalent salt like NaCl. Applying equation 1 gives

$$ (3) \qquad \qquad \begin{equation} \mu\_{NaCl} = \mu\_{NaCl}^o + RT \ln a\_{NaCl}  \end{equation}$$

and substituting equation 2 for $a\_{NaCl}$, we have

$$ (4) \qquad \qquad \begin{equation} \mu\_{NaCl} = \mu\_{NaCl}^o + RT \ln m\_{NaCl} \gamma\_{NaCl} \end{equation}$$

Now, for reasons that are beyond the scope of this post, $\gamma\_{NaCl}$ is *not* the activity coefficient you can look up in tables. What we *can* look up is the mean ionic activity coefficient, which is related to the concentrations of individual ions. So, I need to relate the salt concentration $m\_{NaCl}$ to the individual ion concentrations.

Most salts are **strong electrolytes**, meaning that they dissolve completely[^n] in water to form ions. In the case of NaCl, we can write this as

$$ (5) \qquad \qquad \begin{equation} NaCl => Na^+ + Cl^- \end{equation} $$

In other words, if I dissolve 1 mol of solid $NaCl$ in some water, I don't get 1 mol of dissolved $NaCl$ molecules, I get 1 mole of $Na^+$ and 1 mol of $Cl^-$.

So, how do we relate $\mu\_{NaCl}$ to the chemical potentials of the ions? It turns out that the chemical potential of aqueous NaCl is equal to the **sum of the chemical potentials of the ions** into which it dissolves.

$$ (6) \qquad \qquad \begin{equation} \mu\_{NaCl} = \mu\_{Na^+} + \mu\_{Cl^-} \end{equation}$$

which also implies that 

$$ (7) \qquad \qquad \begin{equation} \mu\_{NaCl}^o = \mu^o\_{Na^+} + \mu^o\_{Cl^-} \end{equation}$$

Using equations 1 - 7, and applying the rules of [math with logarithms](https://en.wikipedia.org/wiki/Logarithm#Product.2C_quotient.2C_power_and_root), we have

$$ (8) \qquad \qquad \begin{equation} \mu\_{NaCl} = \mu^o\_{Na^+} + \mu^o\_{Cl-} + RT \ln a\_{Na^+}a\_{Cl^-} \end{equation}$$

$$ (9) \qquad \qquad \begin{equation} \mu\_{NaCl} = \mu^o\_{NaCl} + RT \ln (m\_{Na^+}\gamma\_{Na^+})(m\_{Cl^-}\gamma\_{Cl^-}) \end{equation}$$

which means that

$$ (10) \qquad \qquad \begin{equation} a\_{NaCl} = (m\_{Na^+}\gamma\_{Na^+})(m\_{Cl^-}\gamma\_{Cl^-}) \end{equation}$$

At this point I've equated the activity of dissolved $NaCl$ with the concentrations and activity coefficints of the two individual ions. Unfortunately, those activity coefficients can't be measured directly.

### Working around single-ion activity coefficients

Since it's not possible to measure $\gamma$ for an individual ion; we can only measure the [geometric mean](https://en.wikipedia.org/wiki/Geometric_mean) activity of the two ions, which is defined (for $NaCl$ ) as

$$ (11) \qquad \qquad \begin{equation} a\_\pm = a\_{NaCl}^{\frac{1}{2}} = (m\_{Na^+} m\_{Cl^-} \gamma\_{Na^+}\gamma\_{Cl^-})^{\frac{1}{2}} \end{equation}$$

Correspondingly, the *mean ionic activity coefficient* and *mean ionic molality* are defined as

$$ (12) \qquad \qquad \begin{equation} \gamma\_\pm = (\gamma\_{Na^+} \gamma\_{Cl^-})^{\frac{1}{2}} \end{equation}$$

and

$$ (13) \qquad \qquad \begin{equation} m\_\pm = (m\_{Na^+} m\_{Cl^-})^{\frac{1}{2}} \end{equation}$$

So, for a monovalent salt like $NaCl$, once I find the mean ionic activity coefficient, I can write the chemical potential of the salt as

$$ (14) \qquad \qquad \begin{equation} \mu\_{NaCl} = \mu^o\_{NaCl} + RT \ln (m\_\pm\gamma\_\pm)^2 \end{equation}$$

Notice that the salt concentration, $m\_{NaCl}$, the individual ion concentrations, $m\_{Na^+}$ and $m\_{Cl^-}$, and the mean ionic molality $m\_{\pm}$ are all equal. This is the case **only because $NaCl$ is a monovalent salt** and it makes it easy to be "sloppy" with concentrations and activity coefficients. To get a more precise understanding of how to handle **all** salts, we need to consider a more general case.

### General case for multivalent salts

Now I'll repeat the above steps for a generic salt, $A\_{\nu\_+} B\_{\nu\_-}$, which dissolves into cation $A$ with charge $ z\_+ $ and anion $B$ with charge $z\_-$: 

$$ (15) \qquad \qquad \begin{equation} A\_{\nu\_+} B\_{\nu\_-} => \nu\_{+} A^{z\_+} + \nu\_{-} B^{z\_-} \end{equation}$$

Because the charge of the cation and the anion may not be equal, one mole of salt $A\_{\nu\_+} B\_{\nu\_-}$ will produce ${\nu\_+}$ moles of cations and ${\nu\_-}$ moles of anions. For example, 

$$ (16) \qquad \qquad \begin{equation} MgCl_2 => 1 Mg^{+2} + 2 Cl^{-} \end{equation}$$

Just as with $NaCl$, the chemical potential of the salt is still the sum of the chemical potentials of individual ions, but this time we have to multiply the potential of each one by the respective number of moles. So, we have:

$$ (17) \qquad \qquad \begin{equation} \mu\_{salt} = \nu\_{+} \mu\_{+} + \nu\_{-} \mu\_{-} \end{equation}$$

$$ (18) \qquad \qquad \begin{equation} \mu\_{salt} = \nu\_{+} (\mu^o\_{+} + RT \ln a\_{+}) + \nu\_{-} ( \mu^o\_{-} + RT a\_{-}) \end{equation}$$

$$ (19) \qquad \qquad \begin{equation} \mu\_{salt}^o = \nu\_{+} \mu^o\_{+} + \nu\_{-}\mu^o\_{-} \end{equation}$$

so that

$$ (20) \qquad \qquad \begin{equation} \mu\_{salt} = \mu^o\_{salt} + RT \ln (a\_{+})^{\nu\_{+} }(a\_{-})^{\nu\_{-}} \end{equation}$$

$$ (21) \qquad \qquad \begin{equation} \mu\_{salt} = \mu^o\_{salt} + RT \ln (m\_+ \gamma\_+)^{\nu\_+} (m\_- \gamma\_-)^{\nu\_-} \end{equation}$$

The mean ionic activity coefficient and the mean ionic molality are therefore:

$$ (22) \qquad \qquad \begin{equation} \gamma\_\pm = (\gamma\_{+}^{\nu\_{+}} \gamma\_{-}^{\nu\_{-}})^{\frac{1}{\nu\_{+} + \nu\_{-}}} \end{equation}$$

and

$$ (23) \qquad \qquad \begin{equation} m_\pm = (m\_{+}^{\nu\_{+}} m\_{-}^{\nu\_{-}})^{\frac{1}{\nu\_{+} + \nu\_{-}}} \end{equation}$$

Just like before, if I can find a value for the mean molal activity coefficient, I can calculate the chemical potential of the salt using

$$ (24) \qquad \qquad \begin{equation} \mu\_{salt} = \mu^o\_{salt} + RT \ln (m\_\pm\gamma\_\pm)^{\nu\_{+} + \nu\_{-}} \end{equation}$$

but because of the stoichiometry in this case, **$m\_\pm$ is not equal to $m\_{salt}$**. If I add 1 mol of magnesium chloride ($MgCl_2$) to water, it dissolves into 1 mol of $Mg^{+2}$ and **2** mol of $Cl^{-}$. In other words,

$$ (25) \qquad \qquad \begin{equation} m\_{salt} = \frac{m\_+}{\nu\_+} = \frac{m\_-}{\nu\_-} \end{equation}$$

Even though I can figure out the individual ion concentrations from Eq. 25, the salt concentration is the most straightforward way to express how much salt is in the water. So, it would be nice to be able to calculate the activity from the mean ionic activity coefficient and $m\_{salt}$.

Eq. 21 and 24 show that

$$ (26) \qquad \qquad \begin{equation} a\_{salt} = (m\_+ \gamma\_+)^{\nu\_+} (m\_- \gamma\_-)^{\nu\_-} = (m\_\pm\gamma\_\pm)^{\nu\_{+} + \nu\_{-}} \end{equation}$$

If I use Eq. 25 to substitute $m\_{+}$ and $m\_{-}$ in terms of $m\_{salt}$, I get

$$ (27) \qquad \qquad \begin{equation} a\_{salt} = \nu\_+^{\nu\_+} \nu\_-^{\nu\_-} (m\_{salt} \gamma\_\pm)^{\nu\_+ + \nu\_-} \end{equation}$$

And this, at last, is the general formula for the activity of any salt as a function of the salt concentration and the mean molal activity coefficient. Some examples of this relationship can be found in Appendix 2.1 of *<a  href="https://www.amazon.com/gp/product/0486422259/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=0486422259&linkCode=as2&tag=rkblogamz-20&linkId=d7121ad5e8a5c6595dacccf4bc10891f">Electrolyte Solutions</a><img src="//ir-na.amazon-adsystem.com/e/ir?t=rkblogamz-20&l=am2&o=1&a=0486422259" width="1" height="1" border="0" alt="" style="border:none !important; margin:0px !important;display:inline" />* by Robinson & Stokes (1968).

---

[^n]: This isn't really true. In fact, even strong electrolytes like $NaCl$ don't completely dissociate. In other words, there *is* such a thing as an $NaCl$ ion pair dissolved in water. See [this presentation](https://www.researchgate.net/publication/305082740_Unified_Thermodynamics_for_All_Concentrations_of_Electrolytes_Based_on_Hydration_and_Partial_Dissociation_Without_Activity_Coefficients_1995-) for some excellent work on the subject.