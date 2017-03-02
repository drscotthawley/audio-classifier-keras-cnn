# audio-classifier-keras-cnn
Audio Classifier in Keras using Convolutional Neural Network

## Dependencies
* Python
* numpy
* keras
* theano or tensorflow (as backends)
* librosa


## Data organization:
Sounds files go into a directory called `Samples/`.  With `Samples`, there are subdirectories which divide up the various classes.
e.g., for the IDMT_GUITAR_EFFECTS database,

`$ ls -F /Samples/
Chorus/  Distortion/  EQ/  FeedbackDelay/  Flanger/  NoFX/  Overdrive/  Phaser/  Reverb/  SlapbackDelay/  Tremolo/  Vibrato/
$`

