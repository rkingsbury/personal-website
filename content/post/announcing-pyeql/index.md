---
title: "Announcing pyEQL – a Python library for solution chemistry"
draft: false
date: 2016-01-23
publishDate: 2019-10-10T01:35:16.075216Z
authors: ["Ryan S. Kingsbury"]
---

I'm pleased to announce that with the recent v0.3 release, one of my side projects - pyEQL - has matured enough that I think others might find it useful. So, it's time for an official announcement!

![pyeql-demo-1](/content/images/2017/10/pyeql-demo-1.png)

### What is it?
<a href="https://github.com/rkingsbury/pyEQL">pyEQL</a> is a Python library that performs calculations related to solution chemistry. It can be used interactively from a command line (as in the image above) or integrated into more complex computer models that require information about chemicals in aqueous solution. When I need to do "back of the envelope" calculations to test out ideas related to my work on desalination and salinity-gradient energy processes, <strong>pyEQL is my "envelope."</strong>

The biggest value I find in pyEQL is that it lets me <strong>focus my mental energy on high-level questions</strong> instead of routine calculations. For example, if I'm trying to identify the best operating pressure for a desalination membrane, I don't want to get bogged down in converting units or calculating osmotic pressures for every scenario. pyEQL can do all that, freeing me to focus on the real question at hand.

### What Can it Do?
<ul>
 	<li>Estimate bulk properties like density and conductivity based on the chemical composition of a solution</li>
 	<li>Calculate activity coefficients for dissolved salts from very low to very high concentrations using the well-known Pitzer model</li>
 	<li>Calculate differences in osmotic pressure, mixing energy, or mixing entropy between two solutions</li>
 	<li>Easily integrate into more complex transport or chemical models</li>
 	<li>Convert between different concentration units (mole fraction, mg/L, mol/kg, etc.)</li>
 	<li>Compute molecular weights of a chemical from a text version of its formula (e.g. 'NaHCO3')</li>
 	<li>Print out a listing of data sources for the parameters used in calculations</li>
</ul>
### Who is it for?

Technical professionals, researchers, engineers, and students that routinely perform calculations involving water-based solutions. If you frequently need to look up properties like density, conductivity, concentrations, or activity corrections of different solutions, pyEQL may save you a lot of effort.

### How did it come about?

I began building pyEQL when I was in the early stages of my work on a <a href="http://www.sciencedirect.com/science/article/pii/S0376738815300181">concentration-gradient based energy storage</a> technology. I was trying to compare how a bunch of different salts might perform in the process, and to be as accurate as possible, that meant that I needed to calculate activity corrections for each one. These calculations can be quite complicated, requiring me to look up a unique set of parameters for each salt, then perform something like 10-15 calculation steps. I knew I couldn't make a spreadsheet handle this much complexity and get the results I was after, so I began coding the calculations in Python.

At first I hoped to find a free solution that could plug in to my Python code that would take care of these calculations for me. <a href="http://wwwbrr.cr.usgs.gov/projects/GWC_coupled/phreeqc/">PHREEQC</a> came close, and is quite powerful for its intended purpose, but it's a bit difficult to incorporate into other models as I was trying to do. Everything else I could find was limited to handling dilute solutions, and I specifically wanted my calculations to be accurate in concentrated ones. So, I decided to build my own library. Since then, I've continued to add features bit by bit, guided primarily by what will support my work at the time.

Up until now, pyEQL has been tailored to my own needs, but I've made it open source in hopes of attracting a community of like-minded people who will help me continue to build it into something really powerful. If you're so inclined, <strong><a href="https://github.com/rkingsbury/pyEQL/issues">please suggest a feature, report a bug, or contribute code to the project!</a></strong>

### Where do I get it?
pyEQL is <a href="https://github.com/rkingsbury/pyEQL">available on GitHub</a>. You can find installation instructions there. You can also <a href="https://pyeql.readthedocs.org/en/latest/">read the documentation</a> first, if you prefer. If you want to have access to the latest and greatest (and possibly unstable) changes, be sure to check out the <a href="https://github.com/rkingsbury/pyEQL/tree/develop/pyEQL">develop branch</a>.

### Disclaimer
pyEQL should still be considered ALPHA quality. <strong>Expect bugs</strong>, and expect things to change / break between releases. It includes an ever-growing set of automatic test suites that check its calculations against experimental data (and I do additional checks manually), but these are mostly limited to things that I've had a particular reason to check. Basically, don't use pyEQL (or any computer model, for that matter) for anything important without validating the calculations you need against a credible source.
