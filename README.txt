Copyright (C) 2010 Michael Murphy <murphy.md@gmail.com>

otftsmkr is a utility to create typscript files for ConTeXt setups from
a list of Adobe .otf files. 


** RUNNING OTFTSMKR

To see it in action, try running

otftsmkr /path/to/my/fonts

For each type family found, you will be asked to specify the style. This
only needs to be done once, since otftsmkr stores the information for
subsequent runs.

What results is a set of typescripts in the directory 'typescripts/'
(must exist!), each defining a given typeface. There are also a set of
test input files, again one for each typeface.

The script works by looking at the filename given to the Adobe font and
using it to determine the features of the font. Luckily, for the most
part Adobe name their fonts in the following way:

<FontFamily>-<FontFeaturesInCamelCase>.otf

For instance, Myriad Pro Light Semiextended Italic has the filename

MyriadPro-LightSemiExtIt.otf

This makes it easy to work out what the features of the typeface are and
map it to the proper macro in ConTeXt. We can even handle optical sizes
properly. At the moment, the optical sizes are mapped into ranges of
point sizes. These are

Caption: 6pt - 8pt
Regular: 9pt - 13pt
Subhead: 14pt - 25pt
Display: 26pt - 48pt (in units of 2pt)


** USING THE FONTS IN CONTEXT

The typescripts need to be put in a place where ConTeXt can find them;
something like $TEXMF/tex/context/third/typescripts. You will have to
regenerate the file database so ConTeXt knows where the fonts are.

context --generate

Since LuaTeX converts the .otf files 'on-the-fly', you'll also need to
let LuaTeX know where your fonts are. Instructions for doing that can be
found at http://wiki.contextgarden.net/Fonts_in_LuaTeX.

Now that both LuaTeX and ConTeXt know where the files are, you can add
the following to your ConTeXt file.

\usetypescript[type-adobe]

This is a generic file that comes with this program, and is used to set
some common settings. Then you will need to load each typescript
containing the fonts you want to use, for example

\usetypescript[type-minionpro]
\usetypescript[type-myriadpro]

Now you need to define the body font. This will look something like

\starttypescript [myfont]
  \definetypeface [myfont] [rm] [serif] [minionpro] [regular]
  \definetypeface [myfont] [ss] [sans]  [myriadpro] [regular]
  \definetypeface [myfont] [tt] [mono]  [cursor]    [default] [rscale=1.163]
  \definetypeface [myfont] [mm] [math]  [palatino]  [default] [rscale=1.050]
\stoptypescript

Both 'cursor' and 'palatino' come with ConTeXt. Note that we have loaded
the 'regular' variant. All typefaces should have this defined, but you
can also have other variants, such as 'condensed', 'semicondensed', and
so on, if you have the necessary font files.

The last part is to set the typeface up as the body font.

\usetypescript[myfont]
\setupbodyfont[myfont,11pt]

To change size in the middle of a document, use

\switchtobodyfont[<pointsize>pt]

Only sizes in the ranges given above will work, and only integer point
sizs (i.e., not 14.4pt, as is normal in ConTeXt). You can still use
'tfd' as normal, for exmaple, but it won't use optical scaling.

The standard alternatives known by ConTeXt are:

tf (Roman), 
sl (Slanted), 
it (Italic), 
bf (Bold), 
bi (Bold Italic), and
bs (Bold Slanted)

Any new alternatives, like 'Light', are concatenated to create new
macros. For instance, 'Light' can be accessed with the command '\li',
Semibold Italic with '\seit', and so on. The list of new commands will
be printed after running otftsmkr, where you will be told to place a set
of commands somewhere in your setup. These are required to use the new
font alternatives.

If you want access to some of the other opentype font features, you can
try the following definitions

% font features
\def\orn{\setfontfeature{ornaments}}
\def\sw{\setfontfeature{swash}\it}
\def\sc{\setfontfeature{smallcaps}}
\def\lnfigures{\setfontfeature{lining}}
\def\tabfigures{\addfontfeaturetofont{tabular}}
\def\lntabfigures{\setfontfeature{tabular}}

for use in your document. Font protrusion is also supported by
default. Use

\setupalign[hanging]

to activate it.

** TESTING

To test out new fonts, try inputting the test suite files.

\input test-minionpro


** COMPATIBILITY

otftsmkr is known to work on the following fonts:

Chaparral Pro
Cronos Pro
Minion Pro
Myriad Pro
Warnock Pro


** BUGS

Probably many. Please find them and let me know!


** LICENSE

This software is licensed under the GPL. See LICENSE.txt for more details.

