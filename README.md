# wikt2dict

Wiktionary translation parser tool for many language editions.

## Very quick start

I created a quick_test.sh script for lazy people to test the Wiktionary extraction right away without having to read the whole README.
Just enter the directory bin after cloning and run

    bash quick_test.sh la

where 'la' is the Wiktionary code of the Latin Wiktionary. You can replace it with another that has been configured.
It might take a few minutes.
The results are in the ```../dat/Wiktionary/Latin/word_pairs``` file. The first four columns contain the word pairs, you can ignore the rest. 

If you have enough storage, try running on the English Wiktionary:

    bash quick_test.sh en

## Installation
1. Checkout the repository from GitHub to a directory with at lot of free space.
The amount of free space required depends on the number and size of Wiktionaries
you want to download (for all 40 you will need about 30GB).

    git clone https://github.com/juditacs/wikt2dict.git

2. Enter the wikt2dict/src directory: 
    
    cd wikt2dict/src

3. Set the PYTHONPATH environment variable to the directory where you are right now:

    export PYTHONPATH=$PYTHONPATH:$(pwd)

4. Enter the root directory of wikt2dict:

    cd ..

5. Create all necessary directories (assuming you are using the default configurations):

    mkdir -p dat/wiktionary dat/triangle log res/langnames

5. You are ready to configure wikt2dict.


## Configuration
1. Set the list of Wiktionaries to work with.
We provide an example configuration file, cfg/main.cfg, with the full
configuration we used.
We also provide a defaults.cfg file with default parameters.
You can leave the configuration as it is if you do not want to parse an additional
Wiktionary.

2. Resource files
There are also example files in the res directory.

res/wikicodes: the list of Wiktionary codes you want to collect translations in.
   Right now this file contains all 50 languages we worked with.
res/wiktionaries.tsv: Wiktionary code - language name mapping for all Wiktionaries 
   you want to parse. This is a tab-separated file.

## Parsing Wiktionaries
You will now download and parse the Latin Wiktionary. The environment is already set up.
We chose the Latin Wiktionary because it is small and for historical reasons, easy 
to understand many words for most European-language speakers.
    
1. Create a langdirs.tsv containing nothing but the entity for the Latin Wiktionary.

    grep Latin res/wiktionaries-full.tsv > res/wiktionaries.tsv

2. Download the Wiktionary:

    cd bin

    bash download_wiktionaries.sh  ../res/wiktionaries.tsv ../dat/wiktionary

After the download finishes,
a Latin directory is created in ../dat/wiktionary and the dump should be there.
For downloading more than one Wiktionaries, you need to specify all languages in 
the langnames.tsv file one language-per-line.

If the download fails for some reason, delete the corrupted file before running it again:
    rm ../dat/wiktionary/Latin/lawiktionary-latest-pages-meta-current.xml

3. Convert to plain text:

    bash wiktionary2text.sh ../res/wiktionaries.tsv ../dat/wiktionary

This command should produce the file: ../dat/wiktionary/Latin/lawiktionary.txt

4. Extract translations using the previously set up configuration:

    python extract_translations.py ../cfg/w2d.cfg la

The last parameter tells wikt2dict to extract the Latin Wiktionary.

5. Repeat the steps 1-4. with at least two other Wiktionaries of your choice. 
Preferrably chose Wiktionaries that have a full section in the cfg/w2d.cfg file.

## Triangulating

Now you are ready to try the triangulating. This is done by calling:

    python triangulate.py ../cfg/w2d.cfg

The output is saved in the ../dat/triangle directory by default. You can change this of course.
You can limit the triangulating to a certain languages.
For example calling:
    python triangulate.py ../cfg/w2d.cfg la

would only run triangulating for triangles that contain Latin and skip the others.

Congratulations, you have successfully finished the test tutorial of wikt2dict.
Please send your feedback to judit@sch.bme.hu.

Please cite:

@inproceedings{Acs:2013,


title={Building basic vocabulary across 40 languages},

author={Judit \'Acs and Katalin Pajkossy and Andr\'as Kornai},

booktitle={Proceedings of the 6th Workshop on Building and Using Comparable Corpora},

year={2013}

}

## Upcoming

4lang coverage

Check out our basic vocabulary at: http://hlt.sztaki.hu/resources/4lang/

You can create statistics of the coverage of 4lang and uroboros by calling:

    cat ../dat/lang/*/res/word_pairs | python fourlang_coverage.py ../res/4lang/coverage

This would take all translations extracted from the Wiktionaries and compute
the coverage of 4lang and uroboros based on each language of 4lang and all of them
combined as well.
The statistics are saved in ../res/4lang/ with the coverage prefix.


