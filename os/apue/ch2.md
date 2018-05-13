### 第二章

#### Unix 标准

1. ISO C 
    ISO C 最先由 ANSI 制定，然后被ISO采纳为国际标准 ISO/IEC 9899:1990
    
2. POSIX
    POSIX(Portable Operator System Inerface) 最初是由 IEEE 制定的标准族。该标准的1988版经修改后递交ISO，也就是最终的 IEEE 1003.1-1990 被ISO采纳为国际标准 ISO/IEC 9945-1:1990，通常称该标准为 POSIX.1 

3. SUS(Single UNIX Specification)
    SUS 是 POSIX.1 标准的一个超集，它定义了一些附加接口从而扩展了POSIX.1 的功能。XSI 是 POSIX.1 的可选项部分，其定义了遵循 XSI 标准的实现必须支持POSIX.1 中的哪些可选部分。 UNIX 商标被 Open Group 所拥有，其使用SUS来定义一系列接口，一个系统要想被称为UNIX系统，其实现就必须支持SUS中定义的接口
    
所以，从 所属关系来说 ISO C < POSIX.1 < SUS(UNIX)


#### 限制
