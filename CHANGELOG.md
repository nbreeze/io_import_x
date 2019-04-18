# Changelog
All notable changes to this project will be documented in this file.

The format is based on
[Keep a Changelog](https://keepachangelog.com/en/1.0.0/).

## [0.27] - 2019-04-18
(Poikilos)
### Changed
* bump version to 2.7 to indicate it works with blender 2.7x as per
  semantic versioning (API changes with every "tenth" of version number
  of blender).
* warning in `__init__.py` about unmaintained upstream littleneo version
* moved files around so "Install Add-on from File" feature will work
  with zip downloaded from GitHub:
  - removed duplicate directX_blender/README which was exactly the same
    as directX_blender/io_directx_bel/README)
  - moved io_directx_bel files to root of repo
  - renamed repo to directxblender (formerly directX_blender) so it is
    using a PEP8 module name
  - changed py files to reflect new module naming (from io_directx_bel
    to directxblender)
* moved Changelog text from README to CHANGELOG.md; changed
  formatting to markdown; corrected some spelling/grammar mistakes;
  improved wording.
* renamed README to README.md; changed formatting to markdown; corrected
  some spelling/grammar mistakes; improved wording
* fix incorrect "blender" property in bl_info (minor revision should be
  only in second part of tuple, not 2nd and 3rd--third should be 0--see
  <https://wiki.blender.org/wiki/Process/Addons/Guidelines/metainfo>).
  Also update required version to 2.66 since that seems to be when
  compatibility broke according to JLouis-B >3 yr old unaccepted pull
  request to littleneo's repo.
* add entry for limemidolin below (list items are commit names)
* add nohup.out to .gitignore

## [git] - 2018-04-12
(limemidolin)
* Reformat code
* Add feature: Do not add unused material if wanted
* Add feature: Load normals file if exist
* Multiple file import
* Fix to work on Blender 2.79

## [0.18] - 2012-01-25
(last littleneo entry)
* code rewrite according to api bmesh changes (me.polygon and uv layers essentially)
* implemented foreachset() call for uv import (faster)

## [0.17] - 2012-01-25
* faster, 60% faster in some case : various loops improvements, infile templates parsing disabled by default
    saw another bottleneck about data chunks as string but will wait for binary support for better point of view.
* interface cosmetics

## [rc 0.16] - 2012-01-23
* committed to svn (and littleneo git as usual)
* corrected a bug about referenced token parenting
* corrected a bug about non parented meshes
* armatures/empties importation enabled by default
* last run importer options are saved in a 'last_run' preset,
  so it can be replayed or saved under another name once a
  particular .x profile has been defined
* tagged script for 2.6.1

## [rc 0.15] - 2012-01-12
* name conversion changed from 5 to 4 digits
* matname, imagename and texturename fixes :
* a path test is made at image import time with any existing data images,
  so a same file cant be loaded twice wathever the naming method, / or \, rel or abs etc
  (bel.material.new() and after line 835)
* image and texture names should be ok now (tested with : incrediblylongname.x)
* materials are replaced accordingly in existing objs when using the 'replace' naming method
* fyi, the Dx exporter has the following inconveniences :
  * split linked faces into individual faces
  * inversed uvmapping (y axis) ?
    -> see testfiles/blender_x_export/incrediblylongname.x

## [git] - 2011-12-31
(and 2011-12-29)
* Cosmetics, code cleaning and optimizations
* bpy.ops.object.select_name methods replaced with
    ob.select = True
    bpy.context.scene.objects.active = ob
* corrected a big bug about tokens info appending in dXtree()
* new bpyname() method in bel module. removed bel.common

## [git] - 2011-12-26
* armature import and bone max. length option

## [git] - 2011-11-23
* contrib candidate :)
* solved some naming cases, added bel methods.
* added experimental option about parenting (no armature yet just empties, when needed)
* a bit faster
* added some test files (empty parenting, armature etc)

## [git] - 2011-11-22
campbell feedback (cont):
* added naming methods as options (default is blender name inc. if name exists)
  me and ob remove() should be ok with special cases (empties, mesh with multi-users)
* improved ui

## [git] - 2011-11-21
campbell feedback:
* converted immutables to tuples : templates_x.py and some other vars.
  http://stackoverflow.com/questions/3340539/why-tuple-is-faster-than-list
* dprint() (console debug) removed, replaced by a inloop tests (a bit faster)
  I'd like to keep it for now for easier debug (eg user feedbacks with 'processing' option)

## [git] - 2011-11-19
* object parenting support. parsing the x tree from roots using import_dxtree()
  actually faster than before

## [git] - 2011-11-16
* weight group import
* improved ui a bit and console logs
* x matrices to blender ones conversion

## [git] - 2011-11-14
* global matrix options
* added messy code about binary (not working)

## [git] - 2011-11-11
* import materials and textures (basics) : uv and image mapped (multitex mode)
  and material created with tex slot if any. alpha should be ok.. ?
* added a smooth options
* better tolerance with faulty .x (upper/lower case of template names)
* token names length from x to blender conversion should be ok also (long name cases)
* corrected a parser pointer error after one array parsing case.
* added more templates (for mat and tex support)
* removed texture files from repo in testfile (tex does not match meshes )
  added some other x files for further tests in binary and compressed format
  ( http://assimp.svn.sourceforge.net/viewvc/assimp/trunk/test/models/X/ )

## [git] - 2011-11-08
* turned into an addon (fork from obj import so unused functions atm)
  enable it in addon, then file > import > directx
* splitted directx parser (io_directx_bel folder) and bel draft
  the bel folder is intended to be located in /scripts/modules (shared components)
  but it's ok in scripts/addons too (tbd)
  bel folder (will) includes anything related to blender data helper (read/write)
* corrected duplicated quotes for x string type

## [git] - 2011-11-07
* uv import
* generic directx token parser. templates items are used to read datas of any token type
  a bit slower but cool since it should support non strict standard directx files
  virtually it can retrieve everything from now supposing the template is know
  by default or given in the file. calls are now like :
```
		nbslots, mats = readToken('MeshMaterialList001') or
		uv = readToken('uv001') or
		nVerts, verts, nFaces, faces = readToken('Hydralisk_backbone1') etc
```
* removed the specific mesh parser the 'rigid' file is the last before mutation to
  generic parser. a bit faster but harder to make evolve or adapt. keep it as a faster
  strict 'branch'
* added some default templates
  goals / wip :
  * to compare template declaration in file and default one.
    so either use the default one (strict) or the .x one (could differ)
  * use by the generic data parser to avoid a parser for each kind of token
* cleaner code (grouping methods, function names, docs etc)
  functions separated from calls
  renamed token dict as tokens etc
* added tweaks at the beginning of the file :
	chunksize = 1024     # size of file streams red in a row
	quickmode = False    # this to only find meshes (no parenting, no other tokens than Mesh ones)
	showtree = False     # display the entire token tree in the console
	showtemplate = True  # display template datas found in file
* added a patch for malformed datas of vertices (meshFaces) :
```
	# patch for malformed datas not completely clear yet ( I guess
	# there's bunch of us when looking at meshface syntax in .x files) :
	# when array members are like 3;v0,v1,v2;,
	# and not like 3;v0;v1;v2;, depending on template declarations.
	# http://paulbourke.net/dataformats/directx/#xfilefrm_Use_of_commas
```
* the script now generates linked faces (was not my fault !)
  - it seems Dx always separate each face :
  so it defines (vert * linked faces) verts for one needed vert
  the readvertices loop now remove duplicates at source
  it uses a verts lookup list to redirect vert id defined in faces

## [git] - 2011-11-06
* vertices and faces imported from each test files
* added some info to test yourself in README
* switched to binary for .x as text to retrieve eol (pointer bugs). should be ok whatever it's win, mac or unix text format,
  also works with mixed eol.
  it seems python 3.1 can't return a 'line' when data.realine() when read mode is 'rb' (U default and universal ? really ? ;) )
  when file has mac eol (\r)
  - -> read(1024) in binary, decode, and replace any \r with \n. yes, it doubles lines for windows and lines value is wrong for now
  - -> but the used pointer value is always ok now whatever the file format and still way faster than a data.tell()
  - see CRCF folder to compare output wispwind.x by format.
* files are still splitted into chunks (1024 B) and readable as lines
* references : added 'user' fields when token is used. users store a reference with their childs but with a '*' tag at chr0.
  the tree reflects the changes
* now read anything and add it to the 'tree'. this includes unknow tokens.
* references are recognized. by reference I mean fields like { cube0 } rather than an inline frame cube0 {
  declaration.
  I don't know if one item can be referenced several time or referenced before declaration
  should be...waiting for a case. for now only one 'parent' token, messages will show up
  multi references to one token if cases arise.
* more permissive syntax : 'frame spam{', 'frame     spam   egg{', 'frame spam egg  {'..
* comments are recognized (inlines ones not done yet, since still no useful data red :) )
* header is red
* found other .x test files here :
  http://www.xbdev.net/3dformats/x/xfileformat.php
  created from 3ds max
* added .x files in repo. line 70 and following to switch.
* some token comes with no names, add a noname<00000> to them
* console gives line number (more useful than char position I guess)

## [Unreleased] - 2011-11-05
(first litleneo entry)
day 0 :
* made some disapointing test with ply (from a speed point of view, else it looks really cool)
* made my own parser
* nothing imported for now, it's more about self-eduction to .x and concept
* but it reads the .x structure and can gather some info
