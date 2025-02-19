---
layout: post
title: One Idea for Architecture Recovery
date: 2014-03-10 17:24:56
subtitle: One Idea for Architecture Recovery
author: dym
header-img:
catalog:   true
tags:
     -算法
---

本文迁移自老博客，原始链接为 <https://seven.blog.ustc.edu.cn/one-idea-for-architecture-recovery/>

Def:a module is defined as a conceptual and arbitrary large collection of consecutive source code fragments with an aggregate name .A module (e.g., M) is considered to be a collection of functions F, data types T , and variables V 
We can represent these tuples using the relations contain, import, and export that
constitute the whole architecture. In the above tuple, Module is "module moduleName ", Relationship is contain, import, or export, and Entity is a typed name that refers to a Function definition, Data type, or Global variable.
we can define an architecture A to be a decomposition of a system into n modules where,            A = {mi = &lt; Fi , Ti ,Vi&gt; | i ∈[1…n]}
Then wen use Apriori algorithm to mine the existing association among entities in the form of frequent itemsets. A sample of the frequent itemsets is shown below:
1 &lt;[V-3 T-42 T-44 T-58] [F-83 F-176 F-646 F-647] 4&gt;
2 &lt;[V-3 T-43 T-44 T-58] [F-83 F-647] 2&gt;
3 &lt;[V-3 F-478 F-649 F-719] [F-647 F-648] 2&gt;
4 &lt;[V-4 T-41 T-42 T-44] [F-83 F-647 F-648] 3&gt;
5 &lt;[V-30 F-552 F-553 F-567] [F-547 F-548] 2&gt;
Each line is a record in the database consisting of an itemset (left), followed by the transactions (baskets), and the itemset support.
Apriori algorithm  used to generate a collection of entities that can be considered as the candidates to be contained in a module, given a seed for that module. We call this collection a domain.
For example, if the whole frequent itemsets F in the system are those 5 records that we presented above, then the domain of function F-83 is as follows:
D(F-83):
{&lt;V-3 4&gt; &lt;T-42 4&gt; &lt;T-44 4&gt; &lt;T-58 4&gt;
&lt;F-176 4&gt; &lt;F-646 4&gt; &lt;F-647 4&gt; &lt;V-4 3&gt;
&lt;T-41 3&gt; &lt;F-648 3&gt; &lt;T-43 2&gt;}
The collected domains are the basis for grouping the entities into modules.
Then we use a domain analysis algorithm examines the association values of the entities in the
domain of each potential main-seed in order to come up with the best candidate domain for the variables of each module.
Finally,(cluster strategy ,maybe k-means or others), a module is built around a core entity, i.e, the main-seed, of that module, and data mining provides a restricted and highly associated domain of values for the variables in that module, as well as a criterion for grouping the entities in that module.

Reference:
Kamran Sartipi Kostas Kontogiannis Farhad Mavaddat. Architectural Design Recovery using Data Mining Techniques. University of Waterloo Dept. of Computer Science1 and, Dept. of Electrical & Computer Engineering2 Waterloo, ON. N2L 3G1 Canada, 2000.

