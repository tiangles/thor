ACLOCAL_AMFLAGS = -I m4
AM_LDFLAGS = @BOOST_LDFLAGS@ @COVERAGE_LDFLAGS@
AM_CPPFLAGS = -I@abs_srcdir@/valhalla @BOOST_CPPFLAGS@ -Igenfiles
AM_CXXFLAGS = -I@abs_srcdir@/valhalla @COVERAGE_CXXFLAGS@ -Igenfiles
LIBTOOL_DEPS = @LIBTOOL_DEPS@
libtool: $(LIBTOOL_DEPS)
	$(SHELL) ./config.status libtool

# things for versioning
pkgconfig_DATA = libvalhalla_thor.pc
EXTRA_DIST = version.sh

# conditional test coverage
if ENABLE_COVERAGE
.PHONY: clean-coverage
clean-coverage:
	-find -name '*.gcda' -exec rm -rf {} \;
	-$(LCOV) --directory $(top_builddir) -z
	-rm -rf coverage.info coverage/

.PHONY: coverage-report
coverage-report: clean-coverage
	-$(MAKE) $(AM_MAKEFLAGS) -k check
	$(MAKE) $(AM_MAKEFLAGS) coverage/index.html

coverage.info:
	$(LCOV) --directory $(top_builddir) --base-directory $(top_builddir) --no-external --capture --output-file $@ --no-checksum --compat-libtool

coverage/index.html: coverage.info
	$(GENHTML) --prefix $(top_builddir) --output-directory $(@D) --title "Test Coverage" --legend --show-details $<


.PHONY: clean-gcno
clean-gcno:
	-find -name '*.gcno' -exec rm -rf {} \;

clean-local: clean-coverage clean-gcno clean-genfiles
else
clean-local: clean-genfiles
endif

clean-genfiles:
	-rm -rf genfiles

# lib valhalla compilation etc
lib_LTLIBRARIES = libvalhalla_thor.la
nobase_include_HEADERS = \
	valhalla/thor/astar.h \
	valhalla/thor/astarheuristic.h \
	valhalla/thor/bidirectional_astar.h \
	valhalla/thor/costmatrix.h \
	valhalla/thor/edgestatus.h \
	valhalla/thor/isochrone.h \
	valhalla/thor/optimizer.h \
	valhalla/thor/map_matcher.h \
	valhalla/thor/multimodal.h \
	valhalla/thor/pathalgorithm.h \
	valhalla/thor/pathinfo.h \
	valhalla/thor/route_matcher.h \
	valhalla/thor/service.h \
	valhalla/thor/trippathbuilder.h \
	valhalla/thor/trip_path_controller.h \
	valhalla/thor/trafficalgorithm.h \
	valhalla/thor/timedistancematrix.h
libvalhalla_thor_la_SOURCES = \
	src/thor/astar.cc \
	src/thor/bidirectional_astar.cc \
	src/thor/costmatrix.cc \
	src/thor/isochrone.cc \
	src/thor/isochrone_action.cc \
	src/thor/map_matcher.cc \
	src/thor/matrix_action.cc \
	src/thor/multimodal.cc \
	src/thor/optimized_route_action.cc \
	src/thor/optimizer.cc \
	src/thor/route_action.cc \
	src/thor/route_matcher.cc \
	src/thor/service.cc \
	src/thor/trace_attributes_action.cc \
	src/thor/trace_route_action.cc \
	src/thor/trippathbuilder.cc \
	src/thor/trip_path_controller.cc \
	src/thor/trafficalgorithm.cc \
	src/thor/timedistancematrix.cc

libvalhalla_thor_la_CPPFLAGS = $(DEPS_CFLAGS) $(VALHALLA_DEPS_CFLAGS) @BOOST_CPPFLAGS@
libvalhalla_thor_la_LIBADD = $(DEPS_LIBS) $(VALHALLA_DEPS_LIBS) $(BOOST_PROGRAM_OPTIONS_LIB) $(BOOST_FILESYSTEM_LIB) $(BOOST_SYSTEM_LIB) $(BOOST_THREAD_LIB) @PROTOC_LIBS@

# tests
check_PROGRAMS = \
	test/edgestatus \
	test/optimizer \
	test/thor_service \
	test/trip_path_controller \
	test/astar
test_edgestatus_SOURCES = test/edgestatus.cc test/test.cc
test_edgestatus_CPPFLAGS = $(DEPS_CFLAGS) $(VALHALLA_DEPS_CFLAGS) @BOOST_CPPFLAGS@
test_edgestatus_LDADD = $(DEPS_LIBS) $(VALHALLA_DEPS_LIBS) @BOOST_LDFLAGS@ libvalhalla_thor.la
test_optimizer_SOURCES = test/optimizer.cc test/test.cc
test_optimizer_CPPFLAGS = $(DEPS_CFLAGS) $(VALHALLA_DEPS_CFLAGS) @BOOST_CPPFLAGS@
test_optimizer_LDADD = $(DEPS_LIBS) $(VALHALLA_DEPS_LIBS) @BOOST_LDFLAGS@ libvalhalla_thor.la
test_thor_service_SOURCES = test/thor_service.cc test/test.cc
test_thor_service_CPPFLAGS = $(DEPS_CFLAGS) $(VALHALLA_DEPS_CFLAGS) @BOOST_CPPFLAGS@
test_thor_service_LDADD = $(DEPS_LIBS) $(VALHALLA_DEPS_LIBS) @BOOST_LDFLAGS@ libvalhalla_thor.la
test_trip_path_controller_SOURCES = test/trip_path_controller.cc test/test.cc
test_trip_path_controller_CPPFLAGS = $(DEPS_CFLAGS) $(VALHALLA_DEPS_CFLAGS) @BOOST_CPPFLAGS@
test_trip_path_controller_LDADD = $(DEPS_LIBS) $(VALHALLA_DEPS_LIBS) @BOOST_LDFLAGS@ libvalhalla_thor.la
test_astar_SOURCES = test/astar.cc test/test.cc
test_astar_CPPFLAGS = $(DEPS_CFLAGS) $(VALHALLA_DEPS_CFLAGS) @BOOST_CPPFLAGS@
test_astar_LDADD = $(DEPS_LIBS) $(VALHALLA_DEPS_LIBS) @BOOST_LDFLAGS@ libvalhalla_thor.la

TESTS = $(check_PROGRAMS)
TEST_EXTENSIONS = .sh
SH_LOG_COMPILER = sh

test: check
