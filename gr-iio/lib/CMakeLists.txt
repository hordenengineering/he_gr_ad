# Copyright 2011,2012 Free Software Foundation, Inc.
#
# This file is part of GNU Radio
#
# GNU Radio is free software; you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation; either version 3, or (at your option)
# any later version.
#
# GNU Radio is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.
#
# You should have received a copy of the GNU General Public License
# along with GNU Radio; see the file COPYING.  If not, write to
# the Free Software Foundation, Inc., 51 Franklin Street,
# Boston, MA 02110-1301, USA.

########################################################################
# Setup library
########################################################################
add_library(gnuradio-iio
  attr_sink_impl.cc
  attr_source_impl.cc
  attr_updater_impl.cc
  dds_control.cc
  device_sink_impl.cc
  device_source_impl.cc
)

target_include_directories(gnuradio-iio
  PUBLIC
  $<INSTALL_INTERFACE:include>
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/../include>
  )

target_link_libraries(gnuradio-iio PUBLIC
  gnuradio-runtime
  ${LIBIIO_LIBRARIES}
  ${GR_VOLK_LIB}
  PRIVATE
  gnuradio-blocks
)

if(LIBAD9361_FOUND)
  target_sources(gnuradio-iio PRIVATE
    fmcomms2_sink_impl.cc
    fmcomms2_source_impl.cc
    fmcomms5_sink_impl.cc
    fmcomms5_source_impl.cc
    pluto_sink_impl.cc
    pluto_source_impl.cc
  )
  target_include_directories(gnuradio-iio PUBLIC ${LIBAD9361_INCLUDE_DIRS})
  target_link_libraries(gnuradio-iio PUBLIC ${LIBAD9361_LIBRARIES})
endif(LIBAD9361_FOUND)

#Add Windows DLL resource file if using MSVC
if(MSVC)
    include(${CMAKE_SOURCE_DIR}/cmake/Modules/GrVersion.cmake)

    configure_file(
        ${CMAKE_CURRENT_SOURCE_DIR}/gnuradio-iio.rc.in
        ${CMAKE_CURRENT_BINARY_DIR}/gnuradio-iio.rc
    @ONLY)

    target_sources(gnuradio-iio PRIVATE
        ${CMAKE_CURRENT_BINARY_DIR}/gnuradio-iio.rc
    )
endif(MSVC)

if(BUILD_SHARED_LIBS)
  GR_LIBRARY_FOO(gnuradio-iio IIO)
endif()
