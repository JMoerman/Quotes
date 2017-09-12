PROJECT (com.github.alonsoenrique.quotes)

CMAKE_MINIMUM_REQUIRED (VERSION 2.6)

LIST (APPEND CMAKE_MODULE_PATH ${CMAKE_SOURCE_DIR}/cmake)
LIST (APPEND CMAKE_MODULE_PATH ${CMAKE_SOURCE_DIR}/cmake/src)

SET (DATADIR "${CMAKE_INSTALL_PREFIX}/share")
SET (PKGDATADIR, "${DATADIR}/com.github.alonsoenrique.quotes")

SET (EXEC_NAME "com.github.alonsoenrique.quotes")
SET (RELEASE_NAME "Quotes for daily inspiration")
SET (VERSION "0.4")
SET (VERSION_INFO "Initial release")

FIND_PACKAGE (PkgConfig)

PKG_CHECK_MODULES (DEPS REQUIRED gtk+-3.0 libsoup-2.4 json-glib-1.0)

ADD_DEFINITIONS (${DEPS_CFLAGS})
LINK_LIBRARIES (${DEPS_LIBRARIES})
LINK_DIRECTORIES (${DEPS_LIBRARIES_DIRS})

FIND_PACKAGE (Vala REQUIRED)
INCLUDE (ValaVersion)
ENSURE_VALA_VERSION ("0.26" MINIMUM)

INCLUDE (ValaPrecompile)
VALA_PRECOMPILE (VALA_C ${EXEC_NAME}
    src/Application.vala
    src/ui/MainWindow.vala
PACKAGES
    gtk+-3.0>=3.12
    libsoup-2.4
    json-glib-1.0
)

include(GResource)

GLIB_COMPILE_RESOURCES (GLIB_RESOURCES_ICONS SOURCE data/icons/icons.gresource.xml)
GLIB_COMPILE_RESOURCES (GLIB_RESOURCES_CSS SOURCE data/css.gresource.xml)

ADD_EXECUTABLE (${EXEC_NAME} ${VALA_C} ${GLIB_RESOURCES_ICONS} ${GLIB_RESOURCES_CSS})

INSTALL (TARGETS ${EXEC_NAME} RUNTIME DESTINATION bin)
INSTALL (FILES ${CMAKE_CURRENT_SOURCE_DIR}/data/com.github.alonsoenrique.quotes.dekstop DESTINATION ${DATADIR}/applications/)
INSTALL (FILES ${CMAKE_CURRENT_SOURCE_DIR}/data/com.github.alonsoenrique.quotes.appdata.xml DESTINATION ${DATADIR}/metainfo/)