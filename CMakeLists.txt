# Copyright (c) 2014 Roberto Raggi <roberto.raggi@gmail.com>
#
# Permission is hereby granted, free of charge, to any person obtaining a copy of
# this software and associated documentation files (the "Software"), to deal in
# the Software without restriction, including without limitation the rights to
# use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of
# the Software, and to permit persons to whom the Software is furnished to do so,
# subject to the following conditions:
#
# The above copyright notice and this permission notice shall be included in all
# copies or substantial portions of the Software.
#
# THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
# IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS
# FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR
# COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER
# IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN
# CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
project(cplusplus)

cmake_minimum_required(VERSION 2.8)

set(CMAKE_CXX_FLAGS "-std=c++11 -O3")

add_executable(pgen tools/pgen.cpp)
add_executable(kwgen tools/kwgen.cpp)

add_executable(cplusplus
cplusplus/AST.cpp
cplusplus/AST.h
cplusplus/ASTVisitor.cpp
cplusplus/ASTVisitor.h
cplusplus/Arena.h
cplusplus/Control.cpp
cplusplus/Control.h
cplusplus/Globals.h
cplusplus/Main.cpp
cplusplus/Names.cpp
cplusplus/Names.h
cplusplus/Parser.cpp
cplusplus/Symbols.cpp
cplusplus/Symbols.h
cplusplus/Token.cpp
cplusplus/Token.h
cplusplus/TranslationUnit.cpp
cplusplus/TranslationUnit.h
cplusplus/Types.cpp
cplusplus/Types.h
cplusplus/Keywords.kwgen
cplusplus/Parser.pgen
# generated files
KeywordsP.h
ParserP.h
)
include_directories("${CMAKE_CURRENT_BINARY_DIR}")
add_dependencies(cplusplus kwgen pgen)

add_custom_command(OUTPUT ParserP.h
COMMAND ${CMAKE_CURRENT_BINARY_DIR}/pgen ${CMAKE_CURRENT_SOURCE_DIR}/cplusplus/Parser.pgen -o ParserP.h
DEPENDS cplusplus/Parser.pgen
COMMENT "Generate ParserP.h")

add_custom_command(OUTPUT KeywordsP.h
COMMAND ${CMAKE_CURRENT_BINARY_DIR}/kwgen < ${CMAKE_CURRENT_SOURCE_DIR}/cplusplus/Keywords.kwgen > KeywordsP.h
DEPENDS cplusplus/Keywords.kwgen
COMMENT "Generate KeywordsP.h")
