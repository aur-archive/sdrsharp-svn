# Maintainer: Boohbah <boohbah at gmail.com>
# Contributor: KJ4EHD ryan_turner

_pkgname=sdrsharp
pkgname=$_pkgname-svn
pkgver=1119
pkgrel=1
pkgdesc="A high performance Software Defined Radio application"
arch=('i686' 'x86_64')
url="http://sdrsharp.com/"
license=('MIT' 'MS-RSL')
depends=('rtl-sdr' 'mono' 'portaudio')
makedepends=('subversion' 'monodevelop' 'icoutils')
source=('sdrsharp.desktop' 'sdrsharp.sh')
md5sums=('6794e8e2808057bfea16c4daec23973f'
         'c149a9113fe43e685b8a8763a1dda884')

_svntrunk=https://subversion.assembla.com/svn/$_pkgname
_svnmod=$_pkgname

build() {
  cd "$srcdir"
  msg "Connecting to SVN server...."

  if [[ -d "$_svnmod/.svn" ]]; then
    (cd "$_svnmod" && svn up -r "$pkgver")
  else
    svn co "$_svntrunk" --config-dir ./ -r "$pkgver" "$_svnmod"
  fi

  msg "SVN checkout done or server timeout"
  msg "Starting build..."

  rm -rf "$srcdir/$_svnmod-build"
  svn export "$srcdir/$_svnmod" "$srcdir/$_svnmod-build"
  cd "$srcdir/$_svnmod-build/trunk"

  # Build
  mdtool build -c:Release SDRSharp.sln
}

package() {
  cd "$srcdir/$_svnmod-build/trunk/Release"
 
  # Modify config
  sed -i '/SDRSharp.SoftRock.SoftRockIO,SDRSharp.SoftRock/d' SDRSharp.exe.config
  sed -i '/SDRSharp.FUNcube.FunCubeIO,SDRSharp.FUNcube/d' SDRSharp.exe.config
  sed -i '/SDRSharp.FUNcubeProPlus.FunCubeProPlusIO,SDRSharp.FUNcubeProPlus/d' SDRSharp.exe.config
  sed -i '/SDRSharp.RTLTCP.RtlTcpIO,SDRSharp.RTLTCP/d' SDRSharp.exe.config
  sed -i '/SDRSharp.SDRIQ.SdrIqIO,SDRSharp.SDRIQ/d' SDRSharp.exe.config
  sed -i 's/<!-- <add key="RTL-SDR \/ USB" value="SDRSharp.RTLSDR.RtlSdrIO,SDRSharp.RTLSDR" \/> -->/<add key="RTL-SDR \/ USB" value="SDRSharp.RTLSDR.RtlSdrIO,SDRSharp.RTLSDR" \/>/' SDRSharp.exe.config

  # Create PNG of icon
  icotool -x "$srcdir/$_svnmod-build/trunk/SDRSharp/mixer.ico"

  # Install
  mkdir -p "$pkgdir/opt/$_pkgname"
  cp -a * "$pkgdir/opt/$_pkgname"

  install -Dm644 ../LICENSE "$pkgdir/opt/$_pkgname"
  install -Dm644 ../LISENSING_SCOPE "$pkgdir/opt/$_pkgname"

  mkdir -p "$pkgdir/usr/share/applications"
  install -Dm644 "$srcdir/$_pkgname.desktop" "$pkgdir/usr/share/applications"

  mkdir -p "$pkgdir/usr/bin"
  install -Dm755 "$srcdir/$_pkgname.sh" "$pkgdir/usr/bin/$_pkgname"
}

# vim:set ts=2 sw=2 et:
