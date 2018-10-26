# Modifier
---------------------------------------


### Introduction
----------------
This is the Wendy Chapman's NegEx algorithm in Java. It allows determining if a certain term (word or group of words) is negated or not and, if so, the negated phrase. 

The input text contains a line for each term for which you want to know if it is negated or not, in the following format:

	identifier TAB term TAB "sentence". 

For example:

	1 TAB cáncer TAB "El paciente no presenta cáncer ni anemia"

The output it provides has the following format:

	identifier TAB term TAB "sentence" TAB modification TAB type_modification

For the previous input example, the output would be:

	1 TAB cáncer TAB "El paciente no presenta cáncer ni anemia" TAB Negated TAB negPhrases

This indicates that the term 'cáncer' appears negated in this sentence, and that the negation phrase ('no') is in the 'negPhrases' config file.

The modification field can take only two values: Negated and Affirmed.
When the modification field is Affirmed, the value of type_modification is always NONE.
When the modification field is Negated, the type_modification field can take the following four values:
* negPhrases
* pseNegPhrases
* postNegPhrases
* conjunctions


### Directory structure
---------



### Usage
---------

java es.cnio.bionlp.modifier.main.Main [options]

Options:
<pre>
  -help	
  	Show the line to execute Modifier and the options.
  -displayon <boolean>
  	Show the messages at the standard output. Default TRUE (show).
  -language <language>
  	SPANISH (default) or ENGLISH.
  -answerOptionYes <boolean>
  	If a pre-UMLS phrase is used as a post-UMLS phrase, for example, pain and fever denied, 
	it will print the negation scope of, in this case, 0 - 2, for an option of yes (TRUE) or 
	print -2 for an option of no (FALSE). Default TRUE.
  -isOuputFileGenerated <boolean>
  	If TRUE, it generates an output file; if FALSE, it generates a List class. Default TRUE.
  -lemmaConfigFiles <boolean>
	Configuration files with lemma (TRUE) or without lemma (FALSE). Default TRUE (with lemma).
  -routeConfigFiles <string>
	Config files folder name. Default: in ../config_files/
  -routeInTextFile <string>
	Name of the input text file. Default: in ../in/in.txt
  -routeOutTextFile <string>
	Name of the output text file. Default: in ../out/callKit.result
</pre>


### Examples
------------

Let's assume an input file "in.txt" in the directory "in" that includes the following line: 

	1	cáncer	"El paciente no presenta cáncer ni anemia"

If we execute all the options that Modifier provides, as in (in Linux, for Windows it is the same but changing "/" to "\\"):
<pre>
java es.cnio.bionlp.modifier.main.Main -displayon true -language SPANISH -answerOptionYes true -isOuputFileGenerated true -lemmaConfigFiles false -routeConfigFiles ../config_files/ -routeInTextFile ../in/in.txt -routeOutTextFile ../out/out.txt
</pre>

This generates an output file in the directory "out" with the following line: 

	1	cáncer	"El paciente no presenta cáncer ni anemia"	Negated	negPhrases


## Contact
----------
If you have any questions, remarks, bug reports, bug fixes or extensions,
I will be happy to hear from you.

Jesús Santamaría (jsantamaria@cnio.es)


### License
-------

MIT License

Copyright (c) 2018 Secretaría de Estado para el Avance Digital (SEAD)

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

