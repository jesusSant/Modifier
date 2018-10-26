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

### Modifiers
-------------
This algorithm allows to determine not only the negation, but also the uncertainty, the doubt (fuzzy logic).
To do this, 4 types of modifiers are distinguished, ordered from highest to lowest degree of modification:
1. Neg-phrases: ausencia de, rechazado, declina, etc.
2. Post-neg-phrases: debe descartarse para, debe ser descartado para, improbable, etc.
3. Pseudo-neg-phrases: sin aumento, sin cambios sospechosos, tengo dudas, etc.
4. Conjunctions: pero, sin embargo, aunque, etc.

Neg-phrases contains the most radical modifiers. That is, it contains modifiers that deny the term in question, and are equivalent to the logical connective of the negation in the propositional logic. For example, if the term were fever, these modifiers would deny this term: absence of fever, without fever, etc.
The next, post-neg-phrases, also indicates denial, but includes some doubt, so it is a little less strict than the previous one.
Pseudo-neg-phrases is very similar to the previous one, but it includes the doubt directly between its possibilities: I am not sure if, I doubt, I have doubts, etc.
Finally, the least restrictive, are in conjunctions, and correspond to the adversative conjunctions. That is, they contradict, partially or totally, the term: however, but, if not, etc. These modifiers, therefore, allow to determine the degree of uncertainty that a term has with respect to the phrase in which it appears.
This can be very useful for applications that use Modifier to, for example, assign a specific weight to the terms according to their 'degree' of modification. For example, a weight of 0.25 could be assigned to the terms modified with (1), 0.50 to those modified with (2), 0.75 to those modified with (3), and 0.85 to those modified with (4).


### Directory structure
---------
The Modifier directory structure corresponds to a unique package nomenclature called *es.cnio.bionlp.modifier*
This allows their 'fully qualified class name' to be unique.
Therefore, all packages are within that structure:
* es/cnio/bionlp/modifier/config_files/eng/with_lemma/: includes the 4 files with the modifiers explained above, in English and lemmatized.
* es/cnio/bionlp/modifier/config_files/eng/without_lemma/: includes the 4 files with the modifiers explained above, in English and without lemma.
* es/cnio/bionlp/modifier/config_files/spa/with_lemma/: includes the 4 files with the modifiers explained above, in Spanish and lemmatized.
* es/cnio/bionlp/modifier/config_files/spa/without_lemma/: includes the 4 files with the modifiers explained above, in Spanish and without lemma.
* es/cnio/bionlp/modifier/in/: includes the input file in.txt
* es/cnio/bionlp/modifier/main/: includes the main class Main.java and the execution JAR file modifier.jar.
* es/cnio/bionlp/modifier/misc/: includes the modification algorithm. 
* es/cnio/bionlp/modifier/out/: includes the output file callKit.result.
* es/cnio/bionlp/modifier/util/: includes utility java classes.


### Usage
---------
To install and compile Modifier you can consult the file *Installation.md*.
In this section we will assume that it has been installed and compiled correctly, and we only show some execution examples.

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


### Execution via JAR file
-----------------------------------
The modifier.jar file allows to execute Modifier directly from a terminal such as cmd, terminator, etc.
To do this, you have to write the following command line (from the directory where modifier.jar is located):
<pre>
java -jar modifier.jar [options]
</pre>
Where *options* are those shown in the 'Usage' section.
For example, if we type:
<pre>
java -jar modifier.jar -help
</pre>
Modifier will show the allowed options, that is:
<pre>
Usage: java es.cnio.bionlp.modifier.main.Main [options]
Options:
   -help                   <>          : Show this message
   -displayon              <boolean>   : Show the messages at the standard output. Default TRUE (show)
   -language               <string>    : Name of the input language. Default Spanish.
   -answerOptionYes        <boolean>   : TRUE (Yes) or FALSE (No). Default: TRUE (Yes)
   -isOuputFileGenerated   <boolean>   : TRUE generate output file, FALSE generate List. Default TRUE.
   -lemmaConfigFiles       <boolean>   : Configuration files with lemma (TRUE) or without lemma (FALSE). Default TRUE (with lemma).
   -routeConfigFiles       <string>    : Config files folder name. Default: in ../config_files/
   -routeInTextFile        <string>    : Name of the input text file. Default: in ../in/in.txt
   -routeOutTextFile       <string>    : Name of the output text file. Default: in ../out/callKit.result
</pre>
The modifier.jar file, as indicated above, is found at src/es/cnio/bionlp/modifier/main/modifier.jar
So, if we move to the 'main' folder and type this:
<pre>
java -jar modifier.jar
</pre>
Modifier will take the default options, and being in the directory structure 'by default' will be executed:
* In the in.txt file, that is in src/es/cnio/bionlp/modifier/in/in.txt
* With the configuration files that are in src/es/cnio/bionlp/modifier/config_files/
* It will generate an output file called callKit.result in src/en/cnio/bionlp/modifier/out/callKit.result

If we change modifier.jar to another directory, we must specify these routes in the options, so that it works correctly.
For example, if we move modifier.jar at the 'es' parent directory, we could execute it in the following manner:
java -jar modifier.jar -routeConfigFiles ./en/cnio/bionlp/modifier/config_files/ -routeInTextFile ./en/cnio/bionlp/modifier/in/in.txt -routeOutTextFile ./out.txt
What will cause it to run with the input file and the previous configuration files, but it will generate an output file at the 'es' parent directory named 'out.txt'.

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

