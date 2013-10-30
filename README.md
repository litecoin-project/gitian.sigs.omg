This repository contains gitian signatures for Bitcoin OMG.  It should be updated on each release.
Random members of the public are encouraged to participate in this process in order to better secure the integrity of the Gitian builds.

## Generating Gitian Sigs

 Checkout bitcoinomg, gitian-builder and gitian.sigs.omg

	mkdir workdir
	cd workdir
	git clone https://github.com/devrandom/gitian-builder.git
	git clone https://github.com/litecoin-project/bitcoinomg.git
	git clone https://github.com/litecoin-project/gitian.sigs.omg.git

 From a directory containing the bitcoinomg source, gitian-builder and gitian.sigs.omg
  
	export SIGNER=(your gitian key, ie bluematt, sipa, etc)
	export VERSION=0.8.0
	cd ./gitian-builder

 Fetch and build inputs: (first time, or when dependency versions change)

	mkdir -p inputs; cd inputs/
	wget 'http://miniupnp.free.fr/files/download.php?file=miniupnpc-1.6.tar.gz' -O miniupnpc-1.6.tar.gz
	wget 'http://www.openssl.org/source/openssl-1.0.1c.tar.gz'
	wget 'http://download.oracle.com/berkeley-db/db-4.8.30.NC.tar.gz'
	wget 'http://zlib.net/zlib-1.2.6.tar.gz'
	wget 'ftp://ftp.simplesystems.org/pub/libpng/png/src/libpng-1.5.9.tar.gz'
	wget 'http://fukuchi.org/works/qrencode/qrencode-3.2.0.tar.bz2'
	wget 'http://downloads.sourceforge.net/project/boost/boost/1.50.0/boost_1_50_0.tar.bz2'
	wget 'http://releases.qt-project.org/qt4/source/qt-everywhere-opensource-src-4.8.3.tar.gz'
	cd ..
	./bin/gbuild ../bitcoinomg/contrib/gitian-descriptors/boost-win32.yml
	mv build/out/boost-win32-1.50.0-gitian2.zip inputs/
	./bin/gbuild ../bitcoinomg/contrib/gitian-descriptors/qt-win32.yml
	mv build/out/qt-win32-4.8.3-gitian-r1.zip inputs/
	./bin/gbuild ../bitcoinomg/contrib/gitian-descriptors/deps-win32.yml
	mv build/out/bitcoinomg-deps-0.0.5.zip inputs/

 Build bitcoind and bitcoin-qt on Linux32, Linux64, and Win32:
  
	./bin/gbuild --commit bitcoinomg=v${VERSION} ../bitcoinomg/contrib/gitian-descriptors/gitian.yml
	./bin/gsign --signer $SIGNER --release ${VERSION} --destination ../gitian.sigs.omg/ ../bitcoinomg/contrib/gitian-descriptors/gitian.yml
	./bin/gbuild --commit bitcoinomg=v${VERSION} ../bitcoinomg/contrib/gitian-descriptors/gitian-win32.yml
	./bin/gsign --signer $SIGNER --release ${VERSION}-win32 --destination ../gitian.sigs.omg/ ../bitcoinomg/contrib/gitian-descriptors/gitian-win32.yml

 It is customary to rename your directories in gitian.sigs.omg to match your github username.  Submit a Pull Request against this repo.  If you are uncertain, please see previous commits for examples.
