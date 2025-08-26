---
{"dg-publish":true,"permalink":"/tool-deep-dives/linux/r-core/","updated":"2024-03-06T14:41:30.000-08:00"}
---

#### R core
- *R core* is a computer language that allows you to do complicated math and data analysis
- Its command-line tool, `rscript`, has a fairly simple usage command that blossoms into a Lovecraftian monster beyond all comprehension
	- `rscript -e 'expression 1' <-e 'expression 2'...> <arguments>`
		- Each `-e` is followed by a mathematical expression in single quotes, `'expression'`
		- The expressions can feed into each other
- I can/should probably dive into this in the future, but for now, I've left a bunch of sources below.






# Metadata

### Sources
[Debian -- Details of package r-base-core in sid](https://packages.debian.org/sid/r-base-core)
[rscript(1): front end for scripting with R - Linux man page](https://linux.die.net/man/1/rscript)
[R: Scripting Front-End for R](https://search.r-project.org/R/refmans/utils/html/Rscript.html)
[R: Run an R script](https://search.r-project.org/CRAN/refmans/callr/html/rscript.html)
### Tags
#tools_linux 