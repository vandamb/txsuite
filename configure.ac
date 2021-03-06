dnl Transmission Suite

AC_INIT([TransmissionSuite],[master],,[txsuite])
AM_INIT_AUTOMAKE([foreign])
AM_MAINTAINER_MODE

BUILDTOOLS_INIT

AC_PROG_CC
AM_PROG_LEX
AC_PROG_YACC

if test x"$prefix" = x"NONE" ; then
	prefix="$ac_default_prefix"
fi

if test x"$exec_prefix" = x"NONE" ; then
	exec_prefix='${prefix}'
fi

top_builddir="`pwd`"

examplesdir='${datarootdir}/examples/${PACKAGE_TARNAME}'
AC_SUBST([examplesdir])

dnl ###### Top-level subprojects

TX_ENABLE_SUBDIR(dss,AS_HELP_STRING([--enable-dss],[build Darwin Streaming Server]),no)
TX_ENABLE_SUBDIR(tsdemux, AS_HELP_STRING([--enable-tsdemux],[don't build the tsdemux utility]),no)
TX_ENABLE_SUBDIR(png2rgb, AS_HELP_STRING([--disable-png2rgb],[don't build the png2rgb utility]))
TX_ENABLE_SUBDIR(tstools, AS_HELP_STRING([--disable-tstools],[don't build the Transport Stream Tools]))
TX_ENABLE_SUBDIR(mpeg4ip, AS_HELP_STRING([--disable-mpeg4ip],[don't build the MPEG4IP suite]))
TX_ENABLE_SUBDIR(mhegc, AS_HELP_STRING([--disable-mhegc],[build the RedButton MHEG compiler/decompiler]),yes)
TX_ENABLE_SUBDIR(mhegenc, AS_HELP_STRING([--enable-mhegenc],[build the SpongeLava MHEG compiler/decompiler]),no)
TX_ENABLE_SUBDIR(schroedinger-tools, AS_HELP_STRING([--disable-vorbis-tools],[don't build the Ogg Vorbis command-line tools]))
TX_ENABLE_SUBDIR(vorbis-tools, AS_HELP_STRING([--disable-vorbis-tools],[don't build the Ogg Vorbis command-line tools]))
TX_ENABLE_SUBDIR(ffmpeg, AS_HELP_STRING([--disable-ffmpeg],[don't build FFmpeg]))
TX_ENABLE_SUBDIR(opencaster, AS_HELP_STRING([--disable-opencaster],[don't build OpenCaster]))

dnl ###### Dependencies

test x"$build_png2rgb" = x"yes" && need_libpng=yes
test x"$build_rbbrowser" = x"yes" && need_libpng=yes

if test x"$need_libpng" = x"yes" ; then
	AC_MSG_CHECKING([for libpng])
	AC_ARG_WITH([libpng],[AS_HELP_STRING([--with-libpng=DIR],[specify libpng installation prefix])],[libpng_config="$withval/bin/libpng-config"],[libpng_config="libpng-config"])
	libpng_prefix=`$libpng_config --prefix 2>/dev/null`
	if test x"$libpng_prefix" = x"" ; then
		have_libpng=no
		AC_MSG_RESULT([not found])
	else
		libpng_version=`$libpng_config --version`
		have_libpng=yes
		AC_MSG_RESULT([$libpng_version installed in $libpng_prefix])
	fi
fi
TX_WITH_INCLUDED_SUBDIR(libpng, AS_HELP_STRING([--with-included-libpng],[use the bundled version of libpng (default=auto)])])

test x"$build_rbbrowser" = x"yes" && need_ffmpeg=yes
TX_CHECK_SUBPROJ_ENABLED([ffmpeg])

test x"$build_mpeg4ip" = x"yes" && need_sdl=yes
TX_WITH_INCLUDED_SUBDIR(SDL, AS_HELP_STRING([--with-included-SDL],[use the bundled version of SDL (default=auto)])])

test x"$build_ffmpeg" = x"yes" && need_x264=yes
TX_WITH_INCLUDED_SUBDIR(x264, AS_HELP_STRING([--with-included-x264],[use the bundled version of x264 (default=auto)])])

test x"$build_ffmpeg" = x"yes" && need_schroedinger=yes
test x"$build_schroedinger_tools" = x"yes" && need_schroedinger=yes
TX_WITH_INCLUDED_SUBDIR(schroedinger, AS_HELP_STRING([--with-included-schroedinger],[use the bundled version of schroedinger (default=auto)])])

test x"$build_schroedinger" = x"yes" && need_liboil=yes
TX_WITH_INCLUDED_SUBDIR(liboil, AS_HELP_STRING([--with-included-liboil],[use the bundled version of liboil (default=auto)])])

test x"$build_vorbis_tools" = x"yes" && need_libvorbis=yes
test x"$build_ffmpeg" = x"yes" && need_libvorbis=yes
TX_WITH_INCLUDED_SUBDIR(libvorbis, AS_HELP_STRING([--with-included-libvorbis],[use the bundled version of libvorbis (default=auto)])])

test x"$build_ffmpeg" = x"yes" && need_libtheora=yes
TX_WITH_INCLUDED_SUBDIR(libtheora, AS_HELP_STRING([--with-included-libtheora],[use the bundled version of libtheora (default=auto)])])

test x"$build_libvorbis" = x"yes" && need_libogg=yes
test x"$build_libtheora" = x"yes" && need_libogg=yes
test x"$build_vorbis_tools" = x"yes" && need_libogg=yes
TX_WITH_INCLUDED_SUBDIR(libogg, AS_HELP_STRING([--with-included-libogg],[use the bundled version of libogg (default=auto)])])

test x"$build_schroedinger_tools" = x"yes" && need_boost=yes
if test x"$need_boost" = x"yes" ; then
	AX_BOOST_DETECT
fi
if test x"$have_boost" = x"no" && test x"$need_boost" = x"yes" ; then
	AC_MSG_ERROR([boost is required to build schroedinger-tools; please specify a path to it with --with-boost=DIR])
fi

test x"$build_vorbis_tools" = x"yes" && want_flac="yes"
TX_WITH_INCLUDED_SUBDIR(flac, AS_HELP_STRING([--with-included-flac],[use the bundled version of flac (default=auto)])])

test x"$build_vorbis_tools" = x"yes" && want_speex="yes"

test x"$build_mhegenc" = x"yes" && need_snacc="yes"
TX_WITH_INCLUDED_SUBDIR(snacc, AS_HELP_STRING([--with-included-snacc],[use the bundled version of snacc (default=auto)])])

TX_BUILD_SUBPROJ(libpng)
TX_BUILD_SUBPROJ(png2rgb)
TX_BUILD_SUBPROJ(SDL)
TX_BUILD_SUBPROJ(mpeg4ip)
TX_BUILD_SUBPROJ(x264)
TX_BUILD_SUBPROJ(libogg)
TX_BUILD_SUBPROJ(libvorbis)
TX_BUILD_SUBPROJ(libtheora)
TX_BUILD_SUBPROJ(flac)
TX_BUILD_SUBPROJ(speex)
TX_BUILD_SUBPROJ(vorbis-tools)
TX_BUILD_SUBPROJ(liboil)
TX_BUILD_SUBPROJ(schroedinger)
TX_BUILD_SUBPROJ(schroedinger-tools)
TX_BUILD_SUBPROJ(tstools)
TX_BUILD_SUBPROJ(dss)
TX_BUILD_SUBDIR(mhegc)
TX_BUILD_SUBPROJ(opencaster)
dnl TX_BUILD_SUBPROJ(ffmpeg,[configure --prefix=$prefix --enable-gpl --enable-version3 --enable-nonfree --disable-ffplay --disable-ffserver --enable-libschroedinger --enable-libtheora --enable-libvorbis --enable-libx264])

export PKG_CONFIG_PATH

AC_CONFIG_FILES([
Makefile
buildtools/Makefile
mhegc/Makefile
mhegc/helloworld/Makefile
])

AC_OUTPUT

AC_MSG_RESULT([

Configuration summary:

Darwin Streaming Server:     $build_dss
RedButton mhegc/mhegd:       $build_mhegc
tsdemux:                     $build_tsdemux
libpng:                      $build_libpng
png2rgb:                     $build_png2rgb
Transport Stream Tools:      $build_tstools
SDL:                         $build_sdl
MPEG4IP:                     $build_mpeg4ip
liboil:                      $build_liboil
Schroedinger:                $build_schroedinger
x264:                        $build_x264
Schroedinger Tools:          $build_schroedinger_tools
Ogg:                         $build_libogg
Vorbis:                      $build_libvorbis
Theora:                      $build_libtheora
FLAC:                        $build_flac
Speex:                       $build_speex
Vorbis Tools:                $build_vorbis_tools
ffmpeg:                      $build_ffmpeg
OpenCaster:                  $build_opencaster
])
