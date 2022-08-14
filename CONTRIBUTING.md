Programming guideline are from here with slight adaptations.
https://www.ssi.gouv.fr/en/guide/rules-for-secure-c-language-software-development/

## 1 Files encoding
The source files are encoded in UTF8 format.
The line feed character is \n (“line feed” in Unix format).

## 2 Code layout and indentation

### 2.1 Maximum lengths
A line of code or comment should not exceed 80 characters.
A line of documentation should not exceed 80 characters.
A file should not exceed 2000 lines (including documentation and comments).
A function should not exceed 200 lines.

### 2.2 Code indentation
The code is indented with spaces: one level of indentation corresponds to 4
space characters. The use of the tab character as an indentation character is
prohibited.
The declaration of variables and their initialisation must be aligned using
indentations.
A space character is systematically left between a keyword and the opening
parenthesis that follows it.
The opening brace of a block is placed on a new line. The block closing brace
is also placed on a new line.
A space character is left before and after each operator.
A space character is left after a comma.
The semicolon indicating the end of a statement is stuck to the last operand of
the statement.
For a function call with many parameters, if it is necessary to place the
parameters on several lines, these parameters are placed on a new line
indented.\
Good example :
```C
...
uint32_t processing_function (
    linked_list_t* p_param1,
    uint32_t ui32_param2,
    const unsigned char* s_param3)
{
    uint32_t ui32_result = 0;
    element_t* pp_out_param4 = NULL;
    if (( NULL == p_param1) || (NULL == s_param3))
    {
        ui32_result = 0;
        goto End;
    }
    ui32_result = function_with_many_params (
        p_param1,
        ui32_param2,
        s_param3,
        pp_out_param4);
    if (1 == ui32_result)
    {
        ...
    }

End:
    return ui32_result;
}
```

## 3 Standard types
If the stdint.h header is present, it must be included in order to benefit from
the integer types that it defines. In its absence, it is necessary to define
the integer types.
If the stdbool.h header is present, it must be included in order to benefit
from the boolean type it defines. In its absence, the bool type must be defined
as presented on the following code (header file from GCC version 4.8.2). The
_Bool type is defined for the compilers compatible with standard C99 and
subsequent standards.

```C
/* Copyright (C) 1998 -2013 Free Software Foundation, Inc. */
RULES FOR SECURE C LANGUAGE SOFTWARE DEVELOPMENT – 161
/*
 * ISO C Standard : 7.16 Boolean type and values <stdbool .h>
 */
# ifndef _STDBOOL_H
# define _STDBOOL_H
# ifndef __cplusplus
# define bool _Bool
# define true 1
# define false 0
#else // __cplusplus
/* Supporting <stdbool .h> in C++ is a GCC extension . */
# define _Bool bool
# define bool bool
# define false false
# define true true
# endif // __cplusplus
/* Signal that all the definitions are present . */
# define __bool_true_false_are_defined 1
# endif // stdbool .h
```

## 4 Naming

### 4.1 Language for implementation
The language used for naming libraries, header files, source files, macros,
types, variables and functions must be English
This use of English avoids mixing words in French with the keywords of the C
language which are in English within the code.
The entire source code produced is thus more coherent.
The language used for documentation and comments should be English from the
beginning of development and for all documentation and comments.

### 4.2 Naming of source file directories
The source files must be organised in libraries. In the case of a large
library, it is recommended to create a tree structure to organise the source
files. The top-level directory must be named with the name of the library.
Sub-directories must be named in such a way that they reflect the criteria for
grouping source files.
The following example shows the organisation of directories for a library
containing utility functions:

Tree structure    | Comment
------------------|--------
utils             | Basic directory of the library
utils/includes    | Directory containing all the header files of the library (API)
utils/collection  | Directory containing the implementation of all collection type data structures (lists, stack, array, hash table, etc.)
utils/concurrency | Directory containing the implementation of mutex, semaphores, conditional variables
utils/threads     | Directory containing the implementation of threads
...               | ...
```

### 4.3 Naming of header files and implementation files
Header files and source files must be prefixed with the name of the library to
which they belong. If the library name is long, it is advisable to use an
abbreviation as a prefix. This abbreviation must be chosen in such a way that
it does not conflict with an already existing library (standard libraries,
third party libraries, etc.).
The following list gives examples of header file and source file names:
utils_linked_list.h, utils_linked_list.c, utils_mutex.h, utils_mutex.c,
utils_thread.h, utils_thread.c ...

### 4.4 Naming of macros
Preprocessor macros must have upper case names. The words making up a macro
name must be separated by the underscore character. The name of a macro should
not match an already existing name of another macro: for example a macro
belonging to a header file of a standard library. The parameters of a macro
must respect the variable naming convention.\
Good example
```C
# define LOG_DEBUG(sMessage) write_log_message(sMessage)
```
The name of a macro, defined to avoid the multiple inclusion of a header file,
uses the name of the header file in upper case. The full stop character is
replaced by an underscore character.\
Good example
```C
# define UTILS_LINKED_LIST_H
```

### 4.5 Naming of types
The name of a type defined using the keyword typedef must be written in lower
case, with the suffix _t. The words making up the type name must be separated
by the underscore character. When defining a type for an enumeration or
structure, the name following the keyword enum or struct must have the suffix
_tag. The name of the type after the closing brace defining the type must be
the same name, with _tag replaced by _t.\
Good example
```C
typedef enum status_tag {
    ...
} status_t;
typedef signed long sint32_t;
typedef struct linked_list_tag
{
    ...
} linked_list_t;
```

### 4.6 Naming of functions
The name of a function must be prefixed by the name (or abbreviation of the
name) of the library to which it belongs. The words making up the name of the
function must be separated by the underscore character. The name of a function
must be written in lower case.\
Good example
```C
status_t utils_linked_list_create (linked_list_t** pp_list);
status_t utils_linked_list_delete (linked_list_t* pp_list);
```

### 4.7 Naming of variables
Variable identifiers will consist of words separated by the underscore
character, without spaces or upper case letters. Each element of the identifier
is used to specify the associated variable (type, sign, size, role, etc.).
The following table shows the prefixes for the variable names according to
type, as well as an example for each type of variable:

Prefix | Variable type                    | Example
-------|----------------------------------|--------
i8     | Signed 8-bit integer             | int8_t i8_byte = 0;
ui8    | Unsigned 8-bit integer           | uint8_t ui8_byte = 0U;
i16    | Signed 16-bit integer            | int16_t i16_option = 0;
ui16   | Unsigned 16-bit integer          | uint16_t ui16_port = 0U;
i32    | Signed 32-bit integer            | int32_t i32_value = 0L;
ui32   | Unsigned 32-bit integer          | uint32_t ui32_counter = 0UL;
i64    | Signed 64-bit integer            | int64_t i64_big_value = 0LL;
ui64   | Unsigned 64-bit integer          | uint64_t ui64_big_counter = 0ULL;
b      | Boolean bool                     | b_is_set = false;
c      | Character char                   | c_letter = ’\0’;
f      | Float float                      | f_value = 0.0f;
d      | Double double                    | d_precised_result = 0.0d;
sz     | Type size_t                      | size_t sz_string_length = 0U;
e      | Enumerated type  variable        | status_t e_status_code = STATUS_ERR;
st     | Structure type variable          | linked_list_t st_list;
a      | Array                            | uint32_t a_values[10];
p      | Pointer type variable            | linked_list_t* p_list = NULL;
pp     | Pointer of pointer type variable | linked_list_t** pp_list = NULL;
s      | String type variable             |char* s_message = NULL;
ws     | String type variable             | in unicode wchar_t* ws_message = NULL;

## 5 Documentation

### 5.1 Format of tags for documentation
The source code documentation must be produced using the Doxygen tool’s tag
system. Doxygen tags must all begin with the @ character. The Doxygen tool also
allows the backslash character. However, in order to have uniformity for the
documentation of the source code, the at-sign prefix for Doxygen commands is
imposed.
A documentation comment begins with `/*! and ends with */`.
The following points outline the minimum documentation that must be present in
a header file.

### 5.2 File header title block
All header files and all source files must begin with a header title block used
to identify:
- the software and / or the library to which the header/source file belongs;
- the company (and if necessary the author) and copyright associated with the
  file;
- the Doxygen @file tag. The @file tag can optionally be followed by the file
  name. In the absence of the file name, the file name is automatically deduced
  from the file in which the @file tag is located.

It is essential to use the @file tag in the header files and the source files.
In its absence the documentation on functions, global variables, type
definitions and enumerations present in the file is not included in the Doxygen
documentation produced.
If the file is part of a library, the command @addtogroup <label> [title] must
be used. This serves to group the documentation of all the functions of a
library within a module in the documentation produced. The label is the name of
the group to be used in all files belonging to the library. The title is
optional. It is used to name the group in the documentation. The @addtogroup
command must be supplemented by the @{ and @} tag pair in order to delimit the
elements of the file belonging to the group.

### 5.3 Documentation of a structure
The definition of a structure must be documented with a comment preceding its
definition. This comment must indicate the role of the structure. Each field of
the structure must be documented.

### 5.4 Documentation of an enumeration
The definition of an enumeration must be documented with a comment preceding
its definition. This comment must indicate in which framework the enumeration
is to be used. Each value in the enumeration must be documented.

### 5.5 Documentation of a global variable
A global variable must be documented with a comment preceding its definition.
This comment must indicate the role of the variable, its initialisation value,
and any invariants that must be respected.

### 5.6 Documentation of a function
The documentation of a function must precede the definition of the function
prototype in the header file. The documentation of a function consists of:
- a brief comment;
- a detailed comment explaining the feature offered by the function;
- the presentation of each parameter, specifying whether it is an input, output
  or both input and output parameter;
- the value returned by the function. In the case of an error code, the success
  case(s) must be indicated, along with the different error codes that can be
  returned and their priority;
- a pre-condition, if any, when the function is called;
- a post-condition, if any, after calling the function;
- any additional remarks or warnings.

Good example
```C
The following lines show the minimum documentation for a header file.
# ifndef UTILS_LINKED_LIST_H
# define UTILS_LINKED_LIST_H

/*!
 * @file linked_list .h
 * @author DEV 1
 *
 * @brief Linked List
 *
 * Function declarations for the manipulation of linked list.
 *
 * @addtogroup utils Library Utils
 * @{
 */

/*!
 * @brief Enumeration of status codes
 *
 * Status codes to indicate the success or the failure of functions
 */
typedef enum status_tag {
    STATUS_SUCCESS = 0,   //!< success
    STATUS_GENERIC_ERROR, //!< generic error
    STATUS_MEMORY_ERROR,  //!< memory allocation error
    STATUS_INVALID_PARAM  //!< invalid parameter
} status_t;

/*!
 * @brief Element of the linked list
 */
typedef struct linked_list_element_tag
{
    struct linked_list_element_tag* pNext;     //!< next element
    struct linked_list_element_tag* pPrevious; //!< previous element
    void* pData;                                //!< data of the element
} linked_list_element_t;

/*!
 * @brief Double linked list
 *
 * Structure to define a double linked list. The type data of the list is void.
 */
typedef struct linked_list_tag
{
    linked_list_element_t* pHead; //!< first element
    linked_list_element_t* pTail; //!< last element
} linked_list_t;

/*!
 * @brief New linked list
 *
 * Creation of a new linked list by allocating the memory for the structure and
 by initializing the list.
 * The new list is empty.
 *
 * @param [out] ppList is the new list
 * @return # STATUS_SUCCESS the creation and the initialization are done with
 success
 * @return # STATUS_INVALID_PARAM if ppList is NULL or
 * if (*ppList ) != NULL
 * @return # STATUS_MEMORY_ERROR fail of the memory allocation
 * @pre ppList != NULL and (*ppList ) == NULL
 * @note the created list has to be deleted
 * by calling #utils_linked_list_delete
 */
status_t utils_linked_list_create (linked_list_t** ppList);

/*!
 * @brief Deletion of the list
 *
 * All the elements of the list are deleted and the used memory is freed .
 * @warning The memory used by the data in the list is not freed ..
 *
 * @param [in, out] ppList the list to delete .
 * @return # STATUS_SUCCESS if the deletion of the list is a success
 * @return # STATUS_INVALID_PARAM if ppList is NULL
 * or if (*ppList ) is NULL
 * @pre ppList != NULL and (*ppList ) != NULL
 * @post (*ppList ) == NULL
 */
status_t utils_linked_list_delete (linked_list_t** ppList);
...

/*! @} */
# endif // UTILS_LINKED_LIST_H
```
