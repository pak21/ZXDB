# ZXDB

ZXDB is an open database containing historical information of software, hardware, magazines and books about ZX-Spectrum and related machines.

It was created by **Einar Saukas**, from the full content of **Martijn van der Heide**'s [WorldOfSpectrum](http://www.worldofspectrum.org/) and **Jim Grimwood**'s [SPOT/SPEX](http://www.users.globalnet.co.uk/~jg27paw4/spot-on/) archives (both imported with consent, directly from their internal files). Afterwards it was expanded with literally tens of thousands of corrections, additions, and integration from many other sources. It's currently the most widely used Sinclair related database, feeding several Spectrum websites and an [open API](https://api.zxinfo.dk/doc/) at [ZXInfo](https://zxinfo.dk/). It's also used as index reference by a dozen different websites and services.

For further details, visit the [ZXDB forum section](https://spectrumcomputing.co.uk/forums/viewforum.php?f=32) at [Spectrum Computing](https://spectrumcomputing.co.uk/).


## Getting Started

Simply download the latest database content, then load it into MySQL/MariaDB:

* `ZXDB_mysql.sql` - The latest complete ZXDB database script for MySQL/MariaDB. That's all you really need!

Optionally you can execute one of the provided scripts to convert file `ZXDB_mysql.sql` above to a different RDBMS:

* `ZXDB_to_SQLServer.ps1` - Powershell script to convert ZXDB into SQL Server compatible T-SQL

* `ZXDB_to_SQLite.py` - Python script to convert ZXDB into SQLite compatible SQL

* `ZXDB_to_generic.groovy` - Groovy script to convert ZXDB into a (more) generic SQL


## Database model

The ZXDB schema is described below:


#### _PRIMARY TABLES_

* `entries` - Spectrum-related items (programs, books, computers and peripherals)

* `labels` - individuals and companies (authors, publishers, development teams, copyright holders)

* `magazines` - published magazines (printed or electronic). The magazine link mask follows this convention:
  * `{i#}` - magazine issue number, with (at least) # digits
  * `{v#}` - magazine issue volume number, with (at least) # digits
  * `{y#}` - magazine issue year, with (at least) # digits
  * `{m#}` - magazine issue month, with (at least) # digits
  * `{M#}` - magazine issue month name, with exactly # letters (starting with uppercase)
  * `{d#}` - magazine issue day, with (at least) # digits
  * `{p#}` - page number, with (at least) # digits
  * `{s#}` - magazine special issue string, preceded by character '#'

* `releases` - each release of an item (date, price, publisher, etc)
  * `release_seq=0` - original release
  * `release_seq=1` - 1st re-release
  * `release_seq=2` - 2nd re-release
  * `...`

* `tools` - Spectrum-related cross-platform utilities and development tools (emulators, compilers, editors, etc)

* `websites` - main websites that provide information about items (WorldOfSpectrum, Tipshop, Wikipedia, etc). The website link mask follows this convention:
  * `{eN}` - entry ID, with (at least) N digits


#### _SECONDARY TABLES_

* `aliases` - alternate titles for items (sometimes generic, sometimes just for a specific release and/or idiom)

* `downloads` - available material related to a specific entry/release (screenshot, tape image, inlay, map, instructions, etc)

* `extras` - obsolete WorldOfSpectrum files, and additional files unrelated to entries or labels (FAQ, AY music, etc)

* `features` - magazine sections that featured certain entry or label references

* `groups` - each group of programs with similar characteristics (participants in the same competition, based on the same original game, etc)

* `hosts` - main services that provide information about certain features

* `interviews` - interviews provided by authors, publishers, etc

* `issues` - each published issue of a magazine

* `labelfiles` - available material related to a specific label (photos, posters, advertisements, etc)

* `licenses` - inspirations or tie-in licenses for certain games (from arcades, books, movies, etc)

* `magfiles` - magazine issues content (electronic magazine files, printed magazine scans, covertape music, etc)

* `nvgs` - oldest files preserved from ftp.nvg.unit.no

* `ports` - Spectrum programs also released on other platforms

* `relatedlinks` - existing pages about Spectrum programs at other main websites

* `remakes` - modern remakes of Spectrum programs

* `scores` - average score received by each entry at main websites

* `toolfiles` - installation files of cross-platform utilities and development tools

* `topics` - catalogue of magazine sections


#### _RELATIONSHIP TABLES_

* `authors` - associate entries to their authors
  * `author_seq=1` - 1st author (or only author)
  * `author_seq=2` - 2nd author
  * `...`

* `booktypeins` - associate typed-in programs to the books that published them

* `compilations` - associate compilations to their list of programs

* `frameworks` - associate utilities (like P.A.W.) to programs authored with them

* `licensors` - associate licenses to their license owners

* `magrefs` - associate entries or labels to pages from magazine issues about them

* `members` - associate groups to their list of programs
  * `series_seq` - only required for sequenced series

* `permissions` - associate labels to distribution permissions granted to websites

* `publishers` - associate entries to their publishers
  * `publisher_seq=1` - 1st publisher of a specific release (or unique publisher)
  * `publisher_seq=2` - 2nd publisher (only if same release has multiple publishers)
  * `...`

* `roles` - associate authors to their roles (for each entry)


#### _ENUMERATION TABLES_

* `availabletypes` - list of availability status for entries:
  * `MIA` - released items not (yet) found/preserved
  * `Available` - released items already preserved
  * `Distribution denied` - items unauthorized for distribution
  * `Distribution denied - still for sale` - items unauthorized for distribution
  * `Never released` - items never released (for whatever reason)
  * `Never released - recovered` - items never officially released, later recovered/preserved

* `countries` - list of countries (using ISO 3166-1 Alpha-2 standard codes)

* `filetypes` - list of file types (screenshot, tape image, inlay, photo, poster, etc)

* `formattypes` - list of file format types (screenshot as SCR or GIF, tape image as TZX or TAP, etc)

* `genretypes` - list of entry types (program type, book type, hardware type, etc)

* `grouptypes` - list of group types:
  * `Series` - programs from the same series, following a specific order
  * `Unsorted Group` - programs from the same collection, but without any specific order
  * `Theme` - programs that share the same theme (Ancient Mythology, Christmas, etc)
  * `Feature` - programs that share the same feature (isometric 3D graphics, AY support, etc)
  * `Major Clone` - programs based on the same original game
  * `Competition` - programs that participated in the same competition
  * `Multiplay Mode` - programs that support a certain multiplayer mode (Cooperative, Teamplay, Versus)
  * `Turn Mode` - programs that support a certain multiplayer turn mode (Alternating, Simultaneous, Turn based)
  * `Control Option` - programs that support a certain control option (Kempston joystick, redefineable keys, etc)

* `idioms` - list of idioms (using ISO 639-1 standard codes)

* `labeltypes` - list of label types (person, nickname, companies)

* `licensetypes` - list of license types (arcade coin-up, book, movie, etc)

* `machinetypes` - list of machine types required for each program:
  * `ZX-Spectrum 16K` - programs that require (at least) 16K
  * `ZX-Spectrum 16K/48K` - programs that work on (at least) 16K, but provide additional features in 48K
  * `ZX-Spectrum 48K` - programs that require (at least) 48K
  * `ZX-Spectrum 48K/128K` - programs that work on (at least) 48K, but provide additional features in 128K (AY music, more levels, etc)
  * `ZX-Spectrum 128K` - programs that require (at least) 128K
  * `ZX-Spectrum 128K (load in USR0 mode)` - programs that require (at least) 128K, and must be loaded in USR0 mode
  * `...`

* `origintypes` - indicates "origin" of certain files (according to Martijn's internal notes)

* `permissiontypes` - permission types:
  * `Allowed` - copyright owner allowed distribution permission for all titles
  * `Denied` - copyright owner denied distribution permission for all titles
  * `Partial` - copyright owner denied distribution permission for some titles only (must check text for further details)
  * `Non-copyright holder` - person or company reported not being copyright owner anymore (must check text for further details)

* `platforms` - list of computer platforms

* `publicationtypes` - possible publication types for certain programs:
  * `As part of a compilation` - program was originally released as part of a compilation
  * `On magazine covertape` - program was originally released on magazine covertape
  * `Type-in` - program was originally released as type-in (from book or magazine)

* `referencetypes` - references from magazines about entries or labels (preview, review, advert, type-in, solution, etc)

* `roletypes` - roles by authors on program development (design, graphics, code, music, etc)

* `schemetypes` - tape protection schemes for programs

* `topictypes` - magazine section types

* `variationtypes` - list of possible item variations (full version, demo, or soundtrack only)


#### _ADDITIONAL DETAILS_

Tables prefixed with `search_by_` are auxiliary tables to help database searches. They are only needed for systems that perform ZXDB searches directly in the database.

Tables prefixed with `spex_` contain information from SPOT/SPEX archive that differs from original WorldOfSpectrum archive, thus pending further investigation later.

Table `sscgc_authors` contains pending information about CSSCGC authors (to be revised soon).

Local file links starting with `/pub/sinclair/` refer to content previously available at the original WorldOfSpectrum archive. These files are currently accessible from [Archive.org](https://archive.org/) mirror at https://archive.org/download/World_of_Spectrum_June_2017_Mirror/World%20of%20Spectrum%20June%202017%20Mirror.zip/World%20of%20Spectrum%20June%202017%20Mirror/sinclair/

Local file links starting with `/zxdb/sinclair/` refer to content added afterwards. These files are currently stored at https://spectrumcomputing.co.uk/zxdb/sinclair/


## Disclaimer

* _Copyright_: ZXDB doesn't contain any copyrighted content. ZXDB is a database containing metadata information only. It doesn't store any kind of copyrighted material internally.

* _Redistribution_: ZXDB cannot grant any rights to redistribute any external copyrighted content. You are welcome to build websites and services using ZXDB but, if you plan to host or provide links for users to download copyrighted material referenced by ZXDB, it's your responsibility to ensure you are allowed to do it.

* _Privacy_: No personal information is stored in ZXDB (email, residence address, etc). ZXDB only stores publicly available information, mainly about authors and publishers (name, official website, etc).

* _Consent_: Whenever ZXDB imported any information from other databases, it was always done with consent from their respective owners, for the purpose of preserving information and improving integration with different websites and services.

* _Validity_: Everybody involved in ZXDB continuously make their best efforts to ensure accuracy of information stored in ZXDB. Even so, ZXDB cannot provide any formal guarantees about the validity of this information. Neither it can be used as base for legal claims about intellectual property or anything else. Use it at your own risk! For a more formal disclaimer, please refer to [Wikipedia disclaimer](https://en.wikipedia.org/wiki/Wikipedia:General_disclaimer).


## License

ZXDB is an open database. Everyone is welcome to contribute and/or use it!

Just please remember to mention somewhere if you used it and, if you decide to create a derived database from ZXDB, please keep it open! For a more formal license model, please refer to ODC [Open Database License](https://en.wikipedia.org/wiki/Open_Database_License) (ODbL 1.0).


## Credits

ZXDB was created and it's maintained by **Einar Saukas**, with very special thanks to many contributors:

* **Dave Hughes**: for directly working on ZXDB, patiently cataloguing each new individual ZX-Spectrum title in ZXDB;

* **Thomas Kolbeck**: for directly working on ZXDB, maintaining the ZX81 section of ZXDB, and implementing the open [ZXInfo API](https://api.zxinfo.dk/doc/);

* **Helga Iliashenko**: for maintaining in [ZX Pokemaster](https://sourceforge.net/projects/zx-pokemaster/) a complete mapping of TOSEC files to their corresponding ZXDB entries (and also providing inumerous other contributions to ZXDB content);

* **Peter Jones**, and **Ricardo Nunes**: for building [Spectrum Computing](https://spectrumcomputing.co.uk/) that hosts the [ZXDB forum section](https://spectrumcomputing.co.uk/forums/viewforum.php?f=32) (and also providing inumerous other contributions to ZXDB content);

* **Hikoki**: for cataloguing all titles released in recent years, and helping to add this information to ZXDB (and also providing inumerous other contributions to ZXDB content);

* Everybody else that have assisted and supported this project since the beginning!

Also special thanks to everyone that contributed to the creation of ZXDB, particularly:

* **Martijn van der Heide**: for creating and maintaining the original [WorldOfSpectrum](http://www.worldofspectrum.org/) archive, and directly helping to import it into ZXDB (clarifying our trickiest questions about the most obscure flags in [WorldOfSpectrum](http://www.worldofspectrum.org/) internal files).

* **Jim Grimwood**: for creating and maintaining the original [SPOT/SPEX](http://www.users.globalnet.co.uk/~jg27paw4/spot-on/) archive.

* **Gerard Sweeney**: for invaluable assistance on importing all original content from the original [WorldOfSpectrum](http://www.worldofspectrum.org/) archive.

* **AndyC**: for reviewing the ZXDB schema, and implementing both SQL Server and SQLite converters.

* **Lee Fogarty**: for providing full access to internal files from the original [WorldOfSpectrum](http://www.worldofspectrum.org/) archive and declaring them as "open source".


## References

The following websites directly use ZXDB internally:

* [Spectrum Computing](https://spectrumcomputing.co.uk/) - ZX-Spectrum archive based on ZXDB, maintained by **Peter Jones** and **Ricardo Nunes**.

* [ZXInfo](https://zxinfo.dk/) - ZX-Spectrum archive based on ZXDB, built with ElasticSearch by **Thomas Kolbeck**.

* [Lisias' Raspberry Pi](http://service.retro.lisias.net/db/) - ZX-Spectrum search engine based on ZXDB, built on Raspberry Pi by **Lisias Toledo**.

* [ZX-Spectrum Reviews (ZXSR)](http://zxspectrumreviews.co.uk/) - ZX-Spectrum Reviews archive by **Chris Bourne**, it now runs ZXSR database integrated with ZXDB.

* [ZX-Art](https://zxart.ee/) - ZX-Spectrum art archive by **Dmitri Ponomarjov**, it includes content imported periodically from ZXDB.

* [ZX Pokemaster](https://sourceforge.net/projects/zx-pokemaster/) - Tool for organizing ZX-Spectrum files by **Helga Iliashenko**, it includes content imported periodically from ZXDB.

* [ZXInfo API](https://api.zxinfo.dk/doc/) - Open ZXDB API, provided by **Thomas Kolbeck**.

The following websites are fully integrated with ZXDB:

* [RZX Archive](http://www.rzxarchive.co.uk/) - Each ZXDB title links to the corresponding webpage at **Daren Pearcy**'s site, and vice-versa.

* [Speccy Screenshot Maps](http://maps.speccy.cz/) - Each ZXDB title links to the corresponding map at **Pavero**'s site, and vice-versa.

* [ZX-Spectrum Reviews (ZXSR)](http://www.zxspectrumreviews.co.uk/) - Each ZXDB title or magazine review links to the corresponding webpage at **Chris Bourne**'s site, and vice-versa.

* [Spectrum 2.0](http://spectrum20.zxdemo.org/) - Each ZXDB title links to the corresponding webpage at **Philip Kendall**'s site.

* [ZX81 Stuff](http://www.zx81stuff.org.uk/) - Each ZXDB title links to the corresponding webpage at **Simon Holdsworth**'s site.

* [Wikipedia](https://en.wikipedia.org/) - Each ZXDB title, person or company links to the corresponding webpage at Wikipedia

* [MobyGames](http://www.mobygames.com/) - Each ZXDB title links to the corresponding webpage at MobyGames.

* [Classic Adventures Solution Archive](http://www.solutionarchive.com/) - Each ZXDB title links to the corresponding webpage at C.A.S.A.

* [Lost in Translation](http://www.exotica.org.uk/) - Each ZXDB title links to the corresponding webpage at Lost in Translation.

* [The Tipshop](http://www.the-tipshop.co.uk/) - Each ZXDB title links to the corresponding webpage at **Gerard Sweeney**'s site.

* [WorldOfSpectrum](http://www.worldofspectrum.org/) - Each ZXDB title links to the corresponding webpage at **Martijn van der Heide**'s site.

* [SPOT/SPEX](http://www.users.globalnet.co.uk/~jg27paw4/spot-on/) - Each ZXDB magazine reference links to the corresponding webpage at **Jim Grimwood**'s site.

* [RZX Archive Channel](https://www.youtube.com/user/rzxarchive) - Each ZXDB title links to the corresponding video at **Daren Pearcy**'s channel.

* [The Spectrum Show](http://www.thespectrumshow.co.uk/) - Each ZXDB title links to the corresponding video at **Paul Jenkinson**'s channel.


![ZXDB](images/ZXDB_8.png)
