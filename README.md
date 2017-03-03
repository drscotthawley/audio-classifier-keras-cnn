# audio-classifier-keras-cnn
Audio Classifier in Keras using Convolutional Neural Network

*ok this is a public repo, but it's not quite ready for public viewing.  Come back later.*

This is an amalgamation of the [Keras MNIST CNN example](https://github.com/fchollet/keras/blob/master/examples/mnist_cnn.py) and [@keunwoochoi](https://github.com/keunwoochoi)'s [CNN Music Tagger](https://github.com/keunwoochoi/music-auto_tagging-keras) -- I would truly say my code is just a "dumbed down" re-hash of Choi's code (that uses his ideas, just not his exact code because it was too clever for me).

## Dependencies
* Python
* numpy
* keras
* theano or tensorflow (as backends)
* librosa


## Data Preparation
### Data organization:
Sound files go into a directory called `Samples/` that is local off wherever the scripts are being run.  Within `Samples`, there are subdirectories which divide up the various classes.

Example: for the guitar sounds in the [IDMT-SMT-Audio-Effects database](https://www.idmt.fraunhofer.de/en/business_units/m2d/smt/audio_effects.html), using their monophonic guitar examples...

    $ ls -F Samples/
    Chorus/  Distortion/  EQ/  FeedbackDelay/  Flanger/   NoFX/  Overdrive/  Phaser/  Reverb/  SlapbackDelay/
    Tremolo/  Vibrato/
    $
(Within each subdirectory of `Samples`, there are loads of .wav or .mp3 files that correspond to each of those classes.)
For now, it's assumed that all data files are exactly the same length (...and probably the same sample rate too...?).  
Also, `librosa` is going to turn stereo files into mono by, e.g. averaging the left & right channels. 

*"Is there any sample data that comes with this repo?"  Not right now, sorry.  So the Samples directory 'ships' as empty.*


### Data preprocessing and/or augmentation:
You don't *have* to preprocess or augment the data.  If you preprocess, it will *much* faster (e.g., 100 times faster reading in the data) if you plan on running more than once. So, preprocess.

The "augmentation" will [vary the speed, pitch, dynamics, etc.](https://bmcfee.github.io/papers/ismir2015_augmentation.pdf) of the sound files ("data") to try to "bootstrap" some extra data with which to train.  It can either be performed *within* the preprocessing step, or you can do it *before* preprocessing as a standalone step (i.e., if you really want to be able to listen to what these augmented datasets sound like). To save disk space, I recommend doing it during preprocessing rather than as a standalone.

If you want to run as a standalone, then you'll run it as
`$ python augment_data <N>  Samples/*/*`
where N is how many augmented copies of each file you want it to create.  It will place all of these in the Samples/ directory with some kind of "_augX" appended to the filename (where X just counts the number of the augmented data files).

Preprocessing will generate mel-spectrograms of all data files, and create a "new version" of `Samples/` called `Preproc/`, with the same subdirectories, but all the .wav and .mp3 files will have ".npy" on the end now.

Then you just run
`$ python preprocess_data.py`

...which currently doesn't DO any data augmentation, but I'm about to add that in *very* soon.


## Training & Evaluating the Network
`$ python train_network.py`
That's all you need.  (I should add command-line arguments to adjust the layer size and number of layers...later.)

It will perform an 80-20 split of training vs. testing data, and give you some validation scores along the way.  

It's set to run for 2000 epochs, feel free to shorten that.

After training, more diagnostics -- ROC curves, AUC -- can be obtained by running

`$ python eval_network.py`

## Results
On the [IDMT Audio Effects Database](https://www.idmt.fraunhofer.de/en/business_units/m2d/smt/audio_effects.html) using the 20,000 monophonic guitar samples across 12 effects classes, this code achieved 99.7% accuracy and an AUC of 0.9999.  This accuracy is comparable to the [original 2010 study by Stein et al.](http://www.ece.rochester.edu/courses/ECE472/resources/Papers/Stein_2010.pdf), who used a Support Vector Machine.

This was achieved by running for 10 hours on [our workstation with an NVIDIA GTX1080 GPU](https://pcpartpicker.com/b/4xLD4D). 

**Future Work**: I just got a new audio dataset I want to try, but it's small and will probably require some augmentation.  And, this github repo and the code iself are still not "cleaned up" for public reading/usage.  Despite being a "public" repo, I'm not publicising this page, just putting it up for a few interested parties.

<hr>
-- [@drscotthawley](https://drscotthawley.github.io), March 2, 2017
