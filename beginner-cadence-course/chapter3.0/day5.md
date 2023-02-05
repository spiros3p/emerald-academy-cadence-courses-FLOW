# Chapter 3 - Day 5 

1.
| entity \ Area | Area 1   | Area 2   | Area 3   | Area 4   |
| ------------- |----------|----------|----------|----------|
|    pub(set)   |   R/W    |   R/W    |   R/W    |   R/W    |
|    pub        |   R/W    |   R/-    |   R/-    |   R/-    |
| acc(contract) |   R/W    |   R/-    |   R/-    |   -/-    |
|   acc(self)   |   R/W    |   -/-    |   -/-    |   -/-    |
|    pub fun    |   yes    |   yes    |   yes    |   yes    |
|  contract fun |   yes    |   yes    |   yes    |   no     |
|  private fun  |   yes    |   no     |   no     |   no     |
