#Installation
Tools needed for this analysis are: samtools, bam-readcount, bowtie, tophat, star, cufflinks, htseq-count, R, cummeRbund, fastqc, picard-tools, and samstat.
In the following installation example the installs are local and will work whether you have root (i.e. admin) access or not
However, if root is available some binaries can/will be copies to system-wide locations (e.g., /usr/bin/)
	
Set up tool installation location

	cd $RNA_HOME
	mkdir tools
	cd tools
	
If for some strange reason, 'wget' is not installed on your Linux or Mac system but curl is.  You can install it as follows:

```
curl -O http://ftp.gnu.org/gnu/wget/wget-1.13.4.tar.gz
tar -xzvf wget-1.13.4.tar.gz
cd wget-1.13.4
./configure --with-ssl=openssl
make
make install
```

##[SAMtools](http://samtools.sourceforge.net/)
Download : http://sourceforge.net/projects/samtools/

	cd $RNA_HOME/tools/
	wget http://sourceforge.net/projects/samtools/files/samtools/0.1.19/samtools-0.1.19.tar.bz2/download -O samtools-0.1.19.tar.bz2
	bunzip2 samtools-0.1.19.tar.bz2 
	tar -xvf samtools-0.1.19.tar
	cd samtools-0.1.19
	make
	./samtools
	
##[bam-readcount](https://github.com/genome/bam-readcount)

	cd $RNA_HOME/tools/
	mkdir git
	cd git
	git clone --recursive git://github.com/genome/bam-readcount.git
	cd $RNA_HOME/tools/
	mkdir bam-readcount
	cd bam-readcount
	export SAMTOOLS_ROOT=$RNA_HOME/tools/samtools-0.1.19
	cmake $RNA_HOME/tools/git/bam-readcount
	make
	./bin/bam-readcount
	
##[Bowtie2](http://bowtie-bio.sourceforge.net/bowtie2/index.shtml)

	cd $RNA_HOME/tools/
	wget http://sourceforge.net/projects/bowtie-bio/files/bowtie2/2.1.0/bowtie2-2.1.0-linux-x86_64.zip
	unzip bowtie2-2.1.0-linux-x86_64.zip
	cd bowtie2-2.1.0
	./bowtie2
	./bowtie2-build
	
##[TopHat2](http://ccb.jhu.edu/software/tophat/index.shtml)

	cd $RNA_HOME/tools/
	wget http://ccb.jhu.edu/software/tophat/downloads/tophat-2.0.8b.Linux_x86_64.tar.gz
	tar -zxvf tophat-2.0.8b.Linux_x86_64.tar.gz
	cd tophat-2.0.8b.Linux_x86_64
	./tophat2
	
##[STAR](https://code.google.com/p/rna-star/)

	cd $RNA_HOME/tools/
	wget http://rna-star.googlecode.com/files/STAR_2.3.0e.Linux_x86_64_static.tgz
	tar -zxvf STAR_2.3.0e.Linux_x86_64_static.tgz
	file STAR_2.3.0e.Linux_x86_64_static/STAR
	
##[Cufflinks2](http://cufflinks.cbcb.umd.edu/)

	cd $RNA_HOME/tools/
	wget http://cufflinks.cbcb.umd.edu/downloads/cufflinks-2.1.1.Linux_x86_64.tar.gz
	tar -zxvf cufflinks-2.1.1.Linux_x86_64.tar.gz
	cd cufflinks-2.1.1.Linux_x86_64
	./cufflinks
	
##[htseq-count](http://www-huber.embl.de/users/anders/HTSeq/doc/count.html)
Download: https://pypi.python.org/packages/source/H/HTSeq/

	cd $RNA_HOME/tools/
	wget https://pypi.python.org/packages/source/H/HTSeq/HTSeq-0.6.1p1.tar.gz
	tar -zxvf HTSeq-0.6.1p1.tar.gz
	cd HTSeq-0.6.1p1/
	python setup.py install --user
	chmod +x scripts/htseq-count
	./scripts/htseq-count
	
##[R](http://www.r-project.org/)
This install takes a while so check if you have R installed already by typing 'which R'
It is already installed on the Cloud, but for completeness, here is how it was done.

```
cd $RNA_HOME/tools/
export R_LIBS=
wget http://cran.r-project.org/src/base/R-3/R-3.1.0.tar.gz
tar -zxvf R-3.1.0.tar.gz
cd R-3.1.0
```

Note, X11 libraries are not available on Amazon cloud so you need use '--with-x=no' during config, on a regular linux system you would not use this option:

```
./configure --with-x=no --prefix=$RNA_HOME/tools/R-3.1.0/
make
make install
./bin/Rscript
```	

##R Libraries
For this tutorial we require:
- [gplots](http://cran.r-project.org/web/packages/gplots/index.html)
- [ggplot2](http://ggplot2.org/)

launch R (enter "R" at command prompt) and type the following at R prompt

```
cd $RNA_HOME/tools/
./R-3.1.0/bin/R
install.packages("gplots")
install.packages("ggplot2")
quit()
```

##[Bioconductor](http://www.bioconductor.org/)
For this tutorial we require:
- [cummeRbund](http://compbio.mit.edu/cummeRbund/)
- [edgeR](http://www.bioconductor.org/packages/release/bioc/html/edgeR.html)

launch R (enter "R" at command prompt) and type the following at R prompt

```
cd $RNA_HOME/tools/
./R-3.1.0/bin/R
source("http://bioconductor.org/biocLite.R")
biocLite("cummeRbund")
biocLite("edgeR")
#If prompted, type "a" to update all old packages
quit()
```	

##[FastQC](http://www.bioinformatics.babraham.ac.uk/projects/fastqc/)
Note, the linux installation below will not work without X11 libraries (as on Amazon cloud)
For the course we will instead install on student laptops
For completeness, here is how it would be installed on a linux system with X11 libraries

```
cd $RNA_HOME/tools/
wget http://www.bioinformatics.babraham.ac.uk/projects/fastqc/fastqc_v0.10.1.zip
unzip fastqc_v0.10.1.zip
cd FastQC/
chmod 755 fastqc
./fastqc --help
```

##[Picard](http://picard.sourceforge.net/command-line-overview.shtml)

	cd $RNA_HOME/tools/
	wget http://sourceforge.net/projects/picard/files/picard-tools/1.114/picard-tools-1.114.zip
	unzip picard-tools-1.114.zip
	java -Xmx2g -jar $RNA_HOME/tools/picard-tools-1.114/MergeSamFiles.jar --help
	
##[SAMStat](http://samstat.sourceforge.net/)

```
cd $RNA_HOME/tools/
wget http://sourceforge.net/projects/samstat/files/latest/download -O samstat.tgz
tar -xzvf samstat.tgz
cd samstat/src
make
```

Note: for some reason samstat will not compile on our Amazon instances.
We were able to compile offline on ubuntu and archlinux
Therefore the instructions above should work in your home lab
For now you can copy a pre-compiled version to your tools dir

	cd $RNA_HOME/tools/
	mkdir samstat
	cp /media/cbwdata/CourseData/RNA_data/samstat $RNA_HOME/tools/samstat/
	$RNA_HOME/tools/samstat/samstat
        #If you get a permissions error, try the following
	chmod +x $RNA_HOME/tools/samstat/samstat
	
##Add locally installed tools to your PATH
To use the locally installed version of each tool without having to specify complete paths, you could add the install directory of each tool to your '$PATH' variable
	
	export RNA_HOME=~/workspace/rnaseq
	export PATH=$RNA_HOME/tools/samtools-0.1.19:$RNA_HOME/tools/bam-readcount/bin:$RNA_HOME/tools/bowtie2-2.1.0:$RNA_HOME/tools/tophat-2.0.8b.Linux_x86_64:$RNA_HOME/tools/STAR_2.3.0e.Linux_x86_64_static:$RNA_HOME/tools/cufflinks-2.1.1.Linux_x86_64:$RNA_HOME/tools/HTSeq-0.6.1p1/scripts:$RNA_HOME/R-3.1.0/bin:$RNA_HOME/tools/FastQC:$RNA_HOME/tools/picard-tools-1.114:$RNA_HOME/tools/samstat:$PATH
	echo $PATH
	
You can make these changes permanent by adding the following lines to your .bashrc file
use a text editor to open your bashrc file. For example:

	vi ~/.bashrc

1. Using your cursor, navigate down to the "export PATH" commands at the end of the file.
2. Delete the line starting with PATH using the vi command "dd".
3. Press the "i" key to enter insert mode. Go to an empty line with you cursor and copy paste the new RNA_HOME and PATH commands into the file
4. Press the "esc" key to exit insert mode.
5. Press the ":" key to enter command mode.
6. Type "wq" to save and quit vi
	
NOTE: If you are worried your .bashrc is messed up you can redownload as follows:

	cd ~
	wget -N https://dl.dropboxusercontent.com/u/16769159/.bashrc
	
	
##Practical Exercise
Try to install bedtools on your own  
Make sure you install it in your tools folder  

	cd $RNA_HOME/tools/

Now, download, unpack, compile, and test  
Hint - google "bedtools" to find the source code  
Hint - there is a README file that will give you hints on how to install  
Hint - If your install has worked you should be able to run bedtools as follows:

	$RNA_HOME/tools/bedtools-2.17.0/bin/bedtools
	