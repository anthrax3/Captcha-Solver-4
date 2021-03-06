# Installing Tesseract 

## Dependencies 

```sh
sudo apt-get install libpng-dev libjpeg-dev libtiff-dev zlib1g-dev
sudo apt-get install gcc g++
sudo apt-get install autoconf automake libtool
sudo apt-get install autoconf-archive zlib1g-dev libtiff5-dev libjpeg8-dev pkg-config libpng12-dev libleptonica-dev
```

For Training Tools 

```sh
sudo apt-get install libicu-dev libpango1.0-dev libcairo2-dev
```

### Leptonica 

[Image Processing Toolkit](http://leptonica.com/) required to build Tesseract. 

#### Installing Leptonica 

```sh
cd "/media/feliz/Safira/GitHub/Captcha-Solver/Tesseract/"
wget https://github.com/DanBloomberg/leptonica/releases/download/1.74.4/leptonica-1.74.4.tar.gz
tar -zxvf leptonica-1.74.4.tar.gz
cd leptonica-1.74.4
./configure
make
sudo checkinstall
sudo ldconfig
```

This installed the Leptonica as a Package, giving it nice properties like install/uninstall using dpkg package manager.
To remove use command `dpkg -r leptonica`

## Tesseract 

Date: 20 August 2017
Version : Tesseract 4.0.0 - alpha

```sh
cd /media/feliz/Safira/GitHub/Captcha-Solver/Tesseract
git clone https://github.com/tesseract-ocr/tesseract.git
cd tesseract
./autogen.sh
./configure
make
sudo make install
sudo ldconfig
make training
sudo make training-install 
sudo ldconfig
```

### Language Data :

```sh
cd tessdata
wget https://github.com/tesseract-ocr/tessdata/raw/4.00/eng.traineddata
sudo mkdir /usr/local/share/tessdata
cd ..
sudo cp -r tessdata/* /usr/local/share/tessdata
```

Add the following line to ~/.bashrc file

```vim
export TESSDATA_PREFIX=/usr/local/share/tessdata
```

# Training Tesseract

## Data Preprocessing

I wrote a small script (available at Preprocessing-Captcha Folder) to change my training images which were in jpeg format to Tiff and also generate the corresponding Box file which is required for Training Tesseract

[Jpeg2Tiff.sh](../Preprocessing-Captcha/jpeg2tiff.sh)

The BoxFile generated by the Tesseract may not be correct hence we would have to edit them (Also following the hint mentioned [Making Box Files - 4.00 Alpha](https://github.com/tesseract-ocr/tesseract/wiki/4.0-with-LSTM#training-tesseract-lstm-engine) we would have to use jTessBoxEditor-2.0-Beta Vesion which has the funtion to add End of Line Tab required by the Tesseract 4.00 Alpha Version) 

Download the [Beta 2.0 of jTessBoxEditor](https://sourceforge.net/projects/vietocr/files/jTessBoxEditor/)

Now For each Training Image, Loading image file tif correct the box coordinates and add End Of Line Tab. 

## Other Required Files

Training Data Files can be looked up at [Data Files Github](https://github.com/tesseract-ocr/tesseract/wiki/Data-Files), note that we require most updated files for Tesseract with LSTM 4.00 Alpha, I used [eng.traineddata](https://github.com/tesseract-ocr/tessdata/raw/4.00/eng.traineddata)



# Reference 

* https://github.com/tesseract-ocr/tesseract/blob/master/INSTALL.GIT.md
* https://github.com/tesseract-ocr/tesseract/wiki/Compiling
* https://www.linux.com/blog/using-tesseract-ubuntu
* https://github.com/tesseract-ocr/tesseract/wiki/Data-Files
* https://github.com/tesseract-ocr/tesseract
* https://github.com/tesseract-ocr/tesseract/wiki/Make-Box-Files
* https://mlichtenberg.wordpress.com/2015/11/04/tuning-tesseract-ocr/
* https://www.youtube.com/watch?v=3Ba7mflTI1I