Same named modules in different project components are indexed just once.

## Steps to reproduce
```
$ cabal --numeric-version
3.2.0.0
$ ghc --numeric-version
8.4.4
$ cd same-named-modules
$ cabal build # compiles 4 project files
$ # delete existing hiedb
$ rm  ~/.local/share/default_*.hiedb
$ hiedb index . # indexes 4 project files
$ hiedb ls # Only shows 2 indexed files
hiedb ls
/home/jhrcek/Devel/github.com/jhrcek/hiedb-sscces/same-named-modules/dist-newstyle/build/x86_64-linux/ghc-8.8.4/same-named-modules-0.1.0.0/x/exe2/build/exe2/exe2-tmp/Other.hie	Other	main
/home/jhrcek/Devel/github.com/jhrcek/hiedb-sscces/same-named-modules/dist-newstyle/build/x86_64-linux/ghc-8.8.4/same-named-modules-0.1.0.0/x/exe2/build/exe2/exe2-tmp/Main.hie	Main	main
```

The issue is that both modules have the same `UnitId`, namely `main`
so only last of the same-named modules is kept due to UNIQUE constraint: https://github.com/wz1000/HieDb/blob/dd4e9f40123a9a06d031a7b00cb70d21e51d530d/src/HieDb/Create.hs#L75
