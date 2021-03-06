macro (build_and_run arg)
	# Add headers to sources to enable file browsing in IDEs
	add_executable(Sqlpp11Sqlite3${arg} ${arg}.cpp ${sqlpp_headers})
	target_link_libraries(Sqlpp11Sqlite3${arg} sqlpp11-connector-sqlite3)
	target_link_libraries(Sqlpp11Sqlite3${arg} "${SQLITE3_LIBRARIES}")
	if (NOT MSVC)
		target_compile_options(Sqlpp11Sqlite3${arg} INTERFACE -Wall -Wextra -pedantic)
	endif ()
	add_test(Sqlpp11Sqlite3${arg} Sqlpp11Sqlite3${arg})
endmacro ()

build_and_run(DateTimeTest)
build_and_run(SampleTest)
build_and_run(SelectTest)
build_and_run(UnionTest)
build_and_run(WithTest)
build_and_run(AttachTest)
build_and_run(DynamicSelectTest)
build_and_run(AutoIncrementTest)
build_and_run(TransactionTest)

# the dynamic loading test needs the extra option "SQLPP_DYNAMIC_LOADING" and does NOT link the sqlite libs
get_filename_component(SQLITE3_LIB_FILE ${SQLITE3_LIBRARIES} NAME)
add_executable(Sqlpp11Sqlite3DynamicLoadingTest "DynamicLoadingTest.cpp" ${sqlpp_headers})
target_link_libraries(Sqlpp11Sqlite3DynamicLoadingTest sqlpp11-connector-sqlite3-dynamic)
if (NOT MSVC)
    target_link_libraries(Sqlpp11Sqlite3DynamicLoadingTest dl)
    target_compile_options(Sqlpp11Sqlite3DynamicLoadingTest INTERFACE -Wall -Wextra -pedantic)
endif ()
add_test(Sqlpp11Sqlite3DynamicLoadingTest Sqlpp11Sqlite3DynamicLoadingTest)
