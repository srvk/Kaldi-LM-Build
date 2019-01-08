# Kaldi Language Model Building #
## Adapting Your Own Language Model ##

[Instructions to learn about building a Kaldi language model based on your own text.](http://speech-kitchen.org/kaldi-language-model-building/)

When you clone this code into a Kaldi experiment like …/kaldi-trunk/egs/tedlium/s5 you get a folder `lm_build/` with tools and examples of how to adapt and train a language model based on your own training text file.

## Adding New Vocabulary Words to the Lexicon

The new script `run_adapt.sh` helps make LM adaptation much easier now.

 *    Method 1: manually create a file `newwords.txt` in the `lm_build` working folder, into which you place new words (not already in the lexicon in `TEDLIUM.152k.dic`) Pronunciations will be automatically generated and added to the dictionary.
 *   Method 2: Automatic candidate OOV words are generated when you run `run_adapt.sh` in the file `candidate_oovs.txt`. This candidate list of new words contains all words found in the training text not already in the dictionary (OOV words)  that appear more than once. Rename this file `newwords.txt` and run `run_adapt.sh` again to use all these words with a frequency greater than 2. Or edit `newwords.txt` having a look at `oov-counts.txt` to see the word frequency counts and help you iteratively refine the dictionary
 *   (optionally) add to the `example_txt` training text file some examples that use the new words. Hint: you may need to repeat these LM adaptation sentences between 50 and 100 times for the transcriber to recognize and produce them as output.
 *   Run the script `run_adapt.sh`. This will do several things, but the end result will be a new composed decoding graph TLG.fst in the output folder `data/lang_phn_test/`
 *   Point your [Eesen Transcriber](http://www.github.com/srvk/eesen-transcriber) setup to use the resulting graph, for example by setting this value in `/vagrant/Makefile.options`

  `GRAPH_DIR?=$(EESEN_ROOT)/asr_egs/tedlium/v2-30ms/lm_build/data/lang_phn_test`

### Adding your own pronunciations
This process makes use of the [CMU Lexicon Tool](http://www.speech.cs.cmu.edu/tools/lextool.html) to generate dictionary entries with phonetic pronunciations for unseen words. These may not always be correct. An alternative approach (Method 3?) Add your own words and pronunciations directly to `TEDLIUM.152k.dic` first - perhaps pattern matching parts of pronunciations from similar words. It is also possible to have more than one pronunciation, e.g:
```
zydeco Z AY D EH K OW
zydeco(2) Z IH D AH K OW
zydeco(3) Z AY D AH K OW
```

### FAQ
[How some of the scripts work](http://speech-kitchen.org/questions-and-answers-about-eesen-tedlium-fbank-lm-building/)  
[Deterministic (tiny) LM Building](https://github.com/srvk/eesen-transcriber/issues/19)  
[Adding Technical Words to Dictionary](https://github.com/srvk/eesen-transcriber/issues/18)  
[More Details About LM Building](https://github.com/srvk/srvk-eesen-offline-transcriber/issues/2)
