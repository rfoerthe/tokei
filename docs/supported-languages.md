# Supported Languages

Tokei supports **320 languages** as of this version. The table below lists each
language's display name and the file extensions (or filenames) used to identify
it. Filenames take priority over extensions when both are defined for the same
language.

To see the list at runtime (with the exact names accepted by `--types`), run:

```shell
tokei --languages
```

To add a new language, see [Contributing](contributing.md).

| Language | Extensions / Filenames |
|----------|------------------------|
| ABAP | `.abap` |
| ABNF | `.abnf` |
| ActionScript | `.as` |
| Ada | `.ada`, `.adb`, `.ads`, `.pad` |
| Agda | `.agda` |
| Alex | `.x` |
| Alloy | `.als` |
| APL | `.apl`, `.aplf`, `.apls` |
| Arduino C++ | `.ino` |
| Arturo | `.art` |
| AsciiDoc | `.adoc`, `.asciidoc` |
| ASN.1 | `.asn1` |
| ASP | `.asa`, `.asp` |
| ASP.NET | `.asax`, `.ascx`, `.asmx`, `.aspx`, `.master`, `.sitemap`, `.webinfo` |
| Assembly | `.asm` |
| GNU Style Assembly | `.s` |
| Astro | `.astro` |
| ATS | `.atxt`, `.dats`, `.hats`, `.sats` |
| Autoconf | `.in` |
| AutoHotKey | `.ahk` |
| Autoit | `.au3` |
| Automake | `.am` |
| AWK | `.awk` |
| Ballerina | `.bal` |
| BASH | `.bash` |
| Batch | `.bat`, `.btm`, `.cmd` |
| Bazel | `.bazel`, `.bzl`, `.bzlmod`, `build`, `module`, `workspace` |
| Bean | `.bean`, `.beancount` |
| Bicep | `.bicep`, `.bicepparam` |
| Bitbake | `.bb`, `.bbappend`, `.bbclass`, `.inc` |
| BQN | `.bqn` |
| BrightScript | `.brs` |
| C | `.c`, `.ec`, `.pgc` |
| Cabal | `.cabal` |
| Cairo | `.cairo` |
| Cangjie | `.cj` |
| Cassius | `.cassius` |
| Ceylon | `.ceylon` |
| Chapel | `.chpl` |
| C Header | `.h` |
| CIL (SELinux) | `.cil` |
| Circom | `.circom` |
| Clojure | `.clj` |
| ClojureC | `.cljc` |
| ClojureScript | `.cljs` |
| CMake | `.cmake`, `cmakelists.txt` |
| COBOL | `.cbl`, `.ccp`, `.cob`, `.cobol`, `.cpy` |
| CodeQL | `.ql`, `.qll` |
| CoffeeScript | `.cjsx`, `.coffee` |
| Cogent | `.cogent` |
| ColdFusion | `.cfm` |
| ColdFusion CFScript | `.cfc` |
| Coq | `.v` |
| C++ | `.c++`, `.cc`, `.cpp`, `.cxx`, `.pcc`, `.tpp` |
| C++ Header | `.hh`, `.hpp`, `.hxx`, `.inl`, `.ipp` |
| Crystal | `.cr` |
| C# | `.cs`, `.csx` |
| C Shell | `.csh` |
| CSS | `.css` |
| CUDA | `.cu` |
| CUE | `.cue` |
| Cython | `.pxd`, `.pxi`, `.pyx` |
| D | `.d` |
| D2 | `.d2` |
| DAML | `.daml` |
| Dart | `.dart` |
| Device Tree | `.dts`, `.dtsi` |
| Dhall | `.dhall` |
| Dockerfile | `.dockerfile`, `.dockerignore`, `dockerfile` |
| .NET Resource | `.resx` |
| Dream Maker | `.dm`, `.dme` |
| Dust.js | `.dust` |
| Ebuild | `.ebuild`, `.eclass` |
| EdgeQL | `.edgeql` |
| Edn | `.edn` |
| 8th | `.8th` |
| Emacs Lisp | `.el` |
| Elixir | `.ex`, `.exs` |
| Elm | `.elm` |
| Elvish | `.elv` |
| Emacs Dev Env | `.ede` |
| Emojicode | `.emojic`, `.🍇` |
| Erlang | `.erl`, `.hrl` |
| EdgeDB Schema Definition | `.esdl` |
| Factor | `.factor` |
| FEN | `.fen` |
| Fennel | `.fnl` |
| Fish | `.fish` |
| FlatBuffers Schema | `.fbs` |
| Forge Config | `.cfg` |
| Forth | `.4th`, `.e4`, `.f83`, `.fb`, `.forth`, `.fpm`, `.fr`, `.frt`, `.ft`, `.fth`, `.rx` |
| FORTRAN Legacy | `.f`, `.f77`, `.for`, `.ftn`, `.pfo` |
| FORTRAN Modern | `.f03`, `.f08`, `.f90`, `.f95`, `.fpp` |
| FreeMarker | `.ftl`, `.ftlh`, `.ftlx` |
| F# | `.fs`, `.fsi`, `.fsscript`, `.fsx` |
| F* | `.fst`, `.fsti` |
| Futhark | `.fut` |
| GDB Script | `.gdb` |
| GDScript | `.gd` |
| GDShader | `.gdshader` |
| Gherkin (Cucumber) | `.feature` |
| Gleam | `.gleam` |
| Glimmer JS | `.gjs` |
| Glimmer TS | `.gts` |
| GLSL | `.comp`, `.frag`, `.geom`, `.glsl`, `.mesh`, `.rahit`, `.rcall`, `.rchit`, `.rgen`, `.rint`, `.rmiss`, `.task`, `.tesc`, `.tese`, `.vert` |
| Gml | `.gml` |
| Go | `.go` |
| Go HTML | `.gohtml` |
| GraphQL | `.gql`, `.graphql` |
| Groovy | `.groovy`, `.grt`, `.gtpl`, `.gvy` |
| Gwion | `.gw` |
| Haml | `.haml` |
| Hamlet | `.hamlet` |
| Handlebars | `.handlebars`, `.hbs` |
| Happy | `.ly`, `.y` |
| Haskell | `.hs` |
| Haxe | `.hx` |
| HCL | `.hcl`, `.tf`, `.tfvars` |
| Headache | `.ha` |
| HEX | `.hex` |
| HICAD | `.MAC`, `.mac` |
| hledger | `.hledger` |
| HLSL | `.fx`, `.fxsub`, `.hlsl` |
| HolyC | `.HC`, `.ZC`, `.hc`, `.zc` |
| HTML | `.htm`, `.html` |
| Hy | `.hy` |
| Idris | `.idr`, `.lidr` |
| INI | `.ini` |
| Intel HEX | `.ihex` |
| Isabelle | `.thy` |
| JAI | `.jai` |
| Janet | `.janet` |
| Java | `.java` |
| JavaScript | `.cjs`, `.js`, `.mjs` |
| Jinja2 | `.j2`, `.jinja` |
| jq | `.jq` |
| JSLT | `.jslt` |
| JSON | `.json` |
| Jsonnet | `.jsonnet`, `.libsonnet` |
| JSX | `.jsx` |
| Julia | `.jl` |
| Julius | `.julius` |
| Jupyter Notebooks | `.ipynb` |
| Just | `.just`, `justfile` |
| K | `.k` |
| Kakoune script | `.kak` |
| Kotlin | `.kt`, `.kts` |
| Korn shell | `.ksh` |
| KV Language | `.kv` |
| LALRPOP | `.lalrpop` |
| Lean | `.hlean`, `.lean` |
| LESS | `.less` |
| Lex | `.l`, `.lex` |
| Lingua Franca | `.lf` |
| LD Script | `.ld`, `.lds` |
| Liquid | `.liquid` |
| Common Lisp | `.asd`, `.lisp`, `.lsp` |
| LiveScript | `.ls` |
| LLVM | `.ll` |
| Logtalk | `.lgt`, `.logtalk` |
| LOLCODE | `.lol` |
| Lua | `.lua`, `.luau` |
| Lucius | `.lucius` |
| M4 | `.m4` |
| Madlang | `.mad` |
| Makefile | `.mak`, `.makefile`, `.mk`, `gnumakefile`, `makefile` |
| Markdown | `.markdown`, `.md` |
| Max | `.maxpat` |
| MDX | `.mdx` |
| Menhir | `.mll`, `.mly`, `.vy` |
| Meson | `meson.build`, `meson_options.txt` |
| Metal Shading Language | `.metal` |
| Mint | `.mint` |
| Mlatu | `.mlt` |
| Modelica | `.mo`, `.mos` |
| Module-Definition | `.def` |
| Mojo | `.mojo`, `.🔥` |
| Monkey C | `.mc` |
| MoonBit | `.mbt`, `.mbti` |
| MoonScript | `.moon` |
| MSBuild | `.csproj`, `.fsproj`, `.props`, `.targets`, `.vbproj` |
| Mustache | `.mustache` |
| Nextflow | `.nextflow`, `.nf` |
| Nim | `.nim` |
| Nix | `.nix` |
| Not Quite Perl | `.nqp` |
| NuGet Config | `nuget.config`, `nugetdefaults.config`, `packages.config` |
| Nushell | `.nu` |
| Objective-C | `.m` |
| Objective-C++ | `.mm` |
| OCaml | `.ml`, `.mli`, `.re`, `.rei` |
| Odin | `.odin` |
| OpenCL | `.cl`, `.ocl` |
| Open Policy Agent | `.rego` |
| OpenQASM | `.qasm` |
| OpenSCAD | `.scad` |
| OpenType Feature File | `.fea` |
| Org | `.org` |
| Oz | `.oz` |
| Pacman's makepkg | `pkgbuild` |
| Pan | `.pan`, `.tpl` |
| Pascal | `.pas` |
| Perl | `.pl`, `.pm` |
| Pest | `.pest` |
| Phix | `.e`, `.exw` |
| PHP | `.php` |
| PlantUML | `.puml` |
| PO File | `.po`, `.pot` |
| Poke | `.pk` |
| Polly | `.polly` |
| Pony | `.pony` |
| PostCSS | `.pcss`, `.sss` |
| PowerShell | `.cdxml`, `.ps1`, `.ps1xml`, `.psc1`, `.psd1`, `.psm1`, `.pssc` |
| Lauterbach PRACTICE Script | `.cmm` |
| Processing | `.pde` |
| Prolog | `.p`, `.pro` |
| Protocol Buffers | `.proto` |
| PRQL | `.prql` |
| PSL Assertion | `.psl` |
| Pug | `.pug` |
| Puppet | `.pp` |
| PureScript | `.purs` |
| Pyret | `.arr` |
| Python | `.py`, `.pyi`, `.pyw` |
| Q | `.q` |
| QCL | `.qcl` |
| QML | `.qml` |
| R | `.r` |
| Racket | `.rkt`, `.scrbl` |
| Rakefile | `.rake`, `rakefile` |
| Raku | `.p6`, `.pl6`, `.pm6`, `.raku`, `.rakumod`, `.rakutest` |
| Razor | `.cshtml`, `.razor` |
| Redscript | `.reds` |
| Ren'Py | `.rpy` |
| ReScript | `.res`, `.resi` |
| ReStructuredText | `.rst` |
| Roc | `.roc` |
| Rusty Object Notation | `.ron` |
| RPM Specfile | `.spec` |
| Ruby | `.rb` |
| Ruby HTML | `.erb`, `.rhtml` |
| Rust | `.rs` |
| Sass | `.sass`, `.scss` |
| Scala | `.sc`, `.scala` |
| Scheme | `.scm`, `.ss` |
| Scons | `sconscript`, `sconstruct` |
| Shell | `.sh` |
| ShaderLab | `.cginc`, `.shader` |
| SIL | `.sil` |
| Slang | `.slang` |
| Slint | `.slint` |
| Smalltalk | `.cs.st`, `.pck.st` |
| Standard ML (SML) | `.sml` |
| Snakemake | `.rules`, `.smk`, `snakefile` |
| Solidity | `.sol` |
| Specman e | `.spe` |
| Spice Netlist | `.ckt` |
| SQF | `.sqf` |
| SQL | `.sql` |
| SRecode Template | `.srt` |
| Stan | `.stan` |
| Stata | `.do` |
| Stratego/XT | `.str` |
| Stylus | `.styl` |
| Svelte | `.svelte` |
| SVG | `.svg` |
| Swift | `.swift` |
| SWIG | `.i`, `.swg` |
| SystemVerilog | `.sv`, `.svh` |
| Tact | `.tact` |
| TCL | `.tcl` |
| Templ | `.templ`, `.tmpl` |
| Tera | `.tera` |
| TeX | `.sty`, `.tex` |
| Plain Text | `.text`, `.txt` |
| Thrift | `.thrift` |
| TOML | `.toml` |
| TSX | `.tsx` |
| TTCN-3 | `.ttcn`, `.ttcn3`, `.ttcnpp` |
| Twig | `.twig` |
| TypeScript | `.cts`, `.mts`, `.ts` |
| Typst | `.typ` |
| Uiua | `.ua` |
| UMPL | `.umpl` |
| Unison | `.u` |
| Unreal Markdown | `.udn` |
| Unreal Plugin | `.uplugin` |
| Unreal Project | `.uproject` |
| Unreal Script | `.uc`, `.uci`, `.upkg` |
| Unreal Shader | `.usf` |
| Unreal Shader Header | `.ush` |
| Ur/Web | `.ur`, `.urs` |
| Ur/Web Project | `.urp` |
| Vala | `.vala` |
| VB6 | `.bas`, `.cls`, `.frm` |
| VBScript | `.vbs` |
| Apache Velocity | `.vm` |
| Verilog | `.vg`, `.vh` |
| Verilog Args File | `.irunargs`, `.xrunargs` |
| VHDL | `.vhd`, `.vhdl` |
| Vim Script | `.vim` |
| Virgil | `.v3` |
| Visual Basic | `.vb` |
| Visual Studio Project | `.vcproj`, `.vcxproj` |
| Visual Studio Solution | `.sln` |
| Vue | `.vue` |
| WebAssembly | `.wast`, `.wat` |
| The WenYan Programming Language | `.wy` |
| WebGPU Shader Language | `.wgsl` |
| Wolfram | `.nb`, `.wl` |
| XAML | `.xaml` |
| Xcode Config | `.xcconfig` |
| XML | `.xml` |
| XSL | `.xsl`, `.xslt` |
| Xtend | `.xtend` |
| YAML | `.yaml`, `.yml` |
| ZenCode | `.zs` |
| Zig | `.zig` |
| ZoKrates | `.zok` |
| Zsh | `.zsh` |
