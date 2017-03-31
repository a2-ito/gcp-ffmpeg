# gcp ffmpeg

````
gcloud compute instances create test --preemptible --zone asia-east1-a --image "ubuntu-1404-trusty-v20170330" --image-project="ubuntu-os-cloud"
````

````
sudo apt-get update
sudo apt-get install -y pkg-config
sudo apt-get install -y yasm
sudo apt-get install -y libmp3lame-dev
sudo apt-get install -y libopus-dev
sudo apt-get install -y libtool
sudo apt-get install -y git
sudo apt-get install -y autoconf

sudo apt-get install -y gcc
sudo apt-get install -y make
sudo apt-get install -y g++

sudo git clone git://git.videolan.org/x264.git
cd x264/
sudo ./configure --enable-static --enable-sared --disable-oencl	
sudo make
sudo make install
sudo ldconfig

sudo git clone --depth 1 git://github.com/mstorsjo/fdk-aac.git
cd fdk-aac
sudo autoreconf -fiv
sudo ./configure --disable-shared
sudo make
sudo make install

sudo git clone https://github.com/FFmpeg/FFmpeg.git
cd FFmpeg
sudo ./configure --enable-gpl --enable-yasm --enable-libx264 --enable-libmp3lame --enable-libopus
sudo make all
sudo make install
````

````
ffmpeg -y -i test.ts \
 -f mp4 -vcodec libx264 \
 -r 30000/1001 -aspect 16:9 -s 1024x768 \
 -bufsize 2000k -maxrate 25000k -ac 2 -ar 48000 \
 -ab 128k -threads 1 -filter_complex channelsplit
````




````
gcloud compute disks snapshot test --snapshot-names ubuntu14-ffmpeg --zone asia-east1-a
````

````
gcloud compute instances delete test
````

snapshot から disk を作成
````
gcloud compute disks create test --source-snapshot ubuntu14-ffmpeg --size 20 --zone asia-east1-a
````

disk から image を作成
````
gcloud compute images create image-ubuntu14-ffmpeg-v20170331 --family image-ubuntu14-ffmpeg --source-disk test --source-disk-zone asia-east1-a
````

disk を削除
````
gcloud compute disks delete test --zone asia-east1-a
````

image から instance を作成
````
gcloud compute instances create test --preemptible --image-family image-ubuntu14-ffmpeg --scopes "https://www.googleapis.com/auth/devstorage.read_write" --zone asia-east1-a
````

compute engine default service アカウントに Cloud Storage の書き込み権限を付与
````
gsutil 
````

