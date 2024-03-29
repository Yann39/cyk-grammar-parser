# Cocke–Younger–Kasami (CYK) Algorithm

Basic **Java Swing** application to demonstrate **grammar parsing** using the **Cocke–Younger–Kasami** algorithm.

![Version](https://img.shields.io/badge/Version-1.0.0-2AAB92.svg)
![Static Badge](https://img.shields.io/badge/Last%20update-12%20Apr%202008-blue)
![Version](https://img.shields.io/badge/Java-6-red.svg)

---

# Table of Contents

* [About the project](#about-the-project)
* [Usage](#usage)
    * [Grammar](#grammar)
    * [Expression](#expression)
    * [File](#file)
* [Technical details](#technical-details)
    * [Pyramid base calculation](#pyramid-base-calculation)
    * [Pyramid upper part calculation](#pyramid-upper-part-calculation)
    * [Tree calculation](#tree-calculation)
* [License](#license)

## About the project

<img alt="Java logo" src="doc/logo-java.svg" height="92"/>

This is a simple GUI application (in French) that demonstrate grammar parsing using the **Cocke–Younger–Kasami** algorithm.

Application has been built on **April 2008** using **Netbeans IDE 5.5**.

It uses the **Swing Application Framework** and was compiled with **Java 6**.

Drawings were built using the **Graphics2D** library.

# Usage

Run the provided `.jar` file :

```bash
java -jar "dist/ProjetLangageFormel.jar"
```

Then just use the UI.
The interface consists of two input areas for entering the **grammar** and the **expression** to check.

![Main interface picture](doc/main_interface.png?raw=true "Main interface")

Once the information is entered, just click on the "Vérifier" (check) button to check if the expression is valid.

If it is valid, a success message will be displayed in the "Résultat" (Result) area as well as two buttons to display the **pyramid** and a preview of the **tree**.

![Pyramid interface picture](doc/pyramid_interface.png?raw=true "Pyramid interface")

![Tree interface picture](doc/tree_interface.png?raw=true "Tree interface")

There is also a button "Importer un fichier" (Import a file) which allows to import a grammar and an expression in the program from a file.
This avoids entering the grammar and expression manually every time. The file must have a specific format described in the next section.
Four example files have been provided in the `samples` directory.

## Grammar

A rule must be entered without spaces or line breaks, and with the following syntax: `left term> right terms`

For example for `S> AB`, `S` is the left term, `A` and `B` are the right terms.

The program does not allow to enter terms of more than 1 character, indeed in the example above, `AB` cannot be considered as a single term but only as two distinct terms `A`
and `B`.

The complete grammar must be entered with a new line after each term, without spaces, by using the following syntax:

```
left term> right terms
left term> right terms
left term> right terms
```

## Expression

The expression to be checked must be entered without spaces or line breaks. Only one expression must be specified.

Example: `abbabaaba`

## File

Importable files in the application must have the following format:

```
#########
Grammar
#########
S> AA
S> AS
S> b
A> SA
A> AS
A> has
##########
Expression
##########
abbabab
```

The grammar and expression input rules apply as well as if they were entered directly in the input fields.

## Technical details

### Pyramid base calculation

The pyramid is actually represented in memory via a two-dimensional array (`String[][]`), following the below scheme about indices :

![Pyramid indices picture](doc/pyramid_indices.png?raw=true "Pyramid indices")

To calculate the base of the pyramid, we call, for each letter in the expression, a function that takes as arguments :

- the list of left terms
- the list of right terms
- the letter

and returns a comma separated list of terms. We store each time the result in the corresponding cell of the pyramid.

### Pyramid upper part calculation

To calculate the upper part of the pyramid, we go trough all the lines (except the base) and all the cells, and apply the following algorithm :

```
for i:=2 to n do
    for j:=1 à n-i+1 do
        for m:=1 à i-1 do
            if B exists in cell (m,j) and C exists in cell (i-m,j+m) so that A->BC € R
                put A in cell (i,j)
        done
    done
done
```

For that we have to :

- find all possible pairs of two strings of type `A, B, C, D`.
- find each of the right terms (separated by commas) that generate a couple.
- remove all duplicates in a string of type `A, B, C, D` in order not to have several times the same value in a cell of the pyramid.

### Tree calculation

Actually it does not generate a real tree but another pyramid with highlighted terms that represent the tree.

Numbers will be displayed in the upper right corner of each cell to indicate the pairs of cells which made possible to find a node of the tree (2 cells with the same number
indicates that they have been used together to find the next node).

This functionality is done using a recursive function, it is applied first to the top of the pyramid, then it is called recursively as soon as it finds a couple of suitable cells.

**Example** :

Following grammar :

```
S>AB A>a C>BA
S>BB B>BB C>AA
A>CC B>CA C>b
A>AB B>b
```

with following expression :

```
bbbabbaaba
```

Give the following pyramid :

![Pyramid example picture](doc/pyramid_example.png?raw=true "Pyramid example")

which corresponds to the following tree :

![Tree example picture](doc/tree_example.png?raw=true "Tree example")

There are multiple possible trees for some grammars, however the program will show always the same tree, as the algorithm always takes the first value that it finds for a couple of
terms.

# License

[General Public License (GPL) v3](https://www.gnu.org/licenses/gpl-3.0.en.html)

This program is free software: you can redistribute it and/or modify it under the terms of the GNU
General Public License as published by the Free Software Foundation, either version 3 of the
License, or (at your option) any later version.

This program is distributed in the hope that it will be useful, but WITHOUT ANY WARRANTY; without
even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the GNU
General Public License for more details.

You should have received a copy of the GNU General Public License along with this program. If not,
see <http://www.gnu.org/licenses/>.