# SPDX-License-Identifier: AGPL-3.0
#
# Maintainer:  Marcell Meszaros < marcell.meszaros AT runbox.eu >
# Maintainer: Pellegrino Prevete <pellegrinoprevete@gmail.com>
# Maintainer: Truocolo <truocolo@aol.com>
# Contributor: Tomasz Maciej Nowak <com[dot]gmail[at]tmn505>
# Contributor: gbr <gbr@protonmail.com>
# Contributor: Maxime Gauduin <alucryd@archlinux.org>
# Contributor: Bartłomiej Piotrowski <bpiotrowski@archlinux.org>
# Contributor: Ionut Biru <ibiru@archlinux.org>
# Contributor: Tom Newsom <Jeepster@gmx.co.uk>
# Contributor: Paul Mattal <paul@archlinux.org>

_pkgname=ffmpeg
pkgname="${_pkgname}5.1"
pkgver=5.1.4
pkgrel=1
epoch=1
_pkgdesc=(
  'Complete solution to record, convert and stream'
  'audio and video (legacy v5.1 branch, with libavcodec v59)')
pkgdesc="${_pkgdesc[*]}"
arch=(
  aarch64
  i686
  x86_64
  arm
  armv7h
  armv6l
  powerpc
  pentium4
)
url="https://${_pkgname}.org"
license=(
  GPL3)
depends=(
  alsa-lib
  aom
  bzip2
  cairo
  dav1d
  fontconfig
  freetype2
  fribidi
  glib2
  glibc
  gmp
  gnutls
  gsm
  jack
  lame
  libass
  libavc1394
  libbluray
  libbs2b
  libdrm
  libgl
  libiec61883
  libmodplug
  libplacebo
  libpulse
  libraw1394
  librsvg
  libsoxr
  libssh
  libtheora
  libva
  libvdpau
  libvorbis
  libvpx
  libwebp
  libx11
  libxcb
  libxext
  libxml2
  libxv
  opencl-icd-loader
  opencore-amr
  opus
  rav1e
  rubberband
  sdl2
  snappy
  srt
  svt-av1
  v4l-utils
  vid.stab
  vulkan-icd-loader
  x264
  x265
  zimg
  zlib
)
depends_x86_64=(
  libmfx)
makedepends=(
  amf-headers
  avisynthplus
  clang
  ffnvcodec-headers
  git
  ladspa
  mesa
  nasm
  opencl-headers
  vulkan-headers
)
optdepends=(
  'avisynthplus: AviSynthPlus support'
  "${_pkgname}: v6.x provides libswresample.so.4, omitted from this ${pkgname} pkg"
  'intel-media-sdk: Intel QuickSync support'
  'ladspa: LADSPA filters'
)
optdepends_x86_64=(
  'nvidia-utils: Nvidia NVDEC/NVENC support'
)
provides=(
  "${_pkgname}-compat-59"
)
conflicts=(
  "${_pkgname}-compat-59"
)
_http="https://git.${_pkgname}.org"
_url="${_http}/${_pkgname}"
source=(
  "git+${_url}.git?signed#tag=n${pkgver}"
)
b2sums=(
  'SKIP')
validpgpkeys=(
  # Michael Niedermayer <michael@niedermayer.cc>
  DD1EC9E8DE085C629B3E1846B18E8928B3948D64
)

prepare() {
  cd \
    "${_pkgname}"
  echo \
    "Applying patches for ffnvcodec SDK 12.1..."
  git \
    cherry-pick \
     -n \
       03823ac0c6a38bd6ba972539e3203a592579792f
  git \
    cherry-pick \
      -n \
        d2b46c1ef768bc31ba9180f6d469d5b8be677500
}

build() {
  cd \
    "${_pkgname}"
  local \
    _build_opts=()
  _build_opts=(
    --prefix=/usr
    --incdir="/usr/include/${pkgname}"
    --libdir="/usr/lib/${pkgname}"
    --disable-autodetect
    --disable-debug
    --disable-doc
    --disable-libopenjpeg
    --disable-libspeex
    --disable-libvmaf
    --disable-libxvid
    --disable-programs
    --disable-sndio
    --disable-static
    --disable-stripping
    # ffmpeg 6.0 & 6.1 contains same major SO version 4
    --disable-swresample
    --enable-alsa
    --enable-amf
    --enable-avisynth
    --enable-bzlib
    --enable-ffnvcodec
    --enable-fontconfig
    --enable-gmp
    --enable-gnutls
    --enable-gpl
    --enable-ladspa
    --enable-libaom
    --enable-libass
    --enable-libbluray
    --enable-libbs2b
    --enable-libdav1d
    --enable-libdc1394
    --enable-libdrm
    --enable-libfreetype
    --enable-libfribidi
    --enable-libgsm
    --enable-libiec61883
    --enable-libjack
    --enable-libmfx
    --enable-libmodplug
    --enable-libmp3lame
    --enable-libopencore_amrnb
    --enable-libopencore_amrwb
    --enable-libopus
    --enable-libplacebo
    --enable-libpulse
    --enable-librav1e
    --enable-librsvg
    --enable-librubberband
    --enable-libsnappy
    --enable-libsoxr
    --enable-libsrt
    --enable-libssh
    --enable-libsvtav1
    --enable-libtheora
    --enable-libv4l2
    --enable-libvidstab
    --enable-libvorbis
    --enable-libvpx
    --enable-libwebp
    --enable-libx264
    --enable-libx265
    --enable-libxcb
    --enable-libxml2
    --enable-libzimg
    --enable-lto
    --enable-opencl
    --enable-opengl
    --enable-sdl2
    --enable-shared
    --enable-v4l2-m2m
    --enable-vaapi
    --enable-vdpau
    --enable-version3
    --enable-vulkan
    --enable-xlib
    --enable-zlib
  )

  [[ $CARCH == "armv7h" ]] || \
  [[ $CARCH == "aarch64" ]] && \
    _build_opts+=(
      --host-cflags="-fPIC")

  [[ $CARCH == "x86_64" ]] && \
    _build_opts+=(
      --enable-cuda-llvm
      --enable-libmfx
      --enable-nvdec
      --enable-nvenc
    )

  ./configure \
    "${_build_opts[@]}"
  make
}

package() {
  make \
    DESTDIR="${pkgdir}" \
    -C "${_pkgname}" \
    install
  cd \
    "${pkgdir}"
  echo \
    "Moving libs to /usr/lib, except the .so symlinks..."
  local \
    _file
  for _file in "usr/lib/${pkgname}/"*; do
    if [[ "${_file}" == *.so ]]; then
      ln \
        -srfv -- \
        usr/lib/"$( \
          readlink \
            "${_file}")" \
        "${_file}"
    elif [[ ! -d "${_file}" ]]; then
      mv \
        -v \
        "${_file}" \
        usr/lib
    fi
  done

  echo \
    "Removing unneeded docs / example files..."
  rm \
    -rv \
    usr/share
}

# vim:set sw=2 sts=-1 et:
