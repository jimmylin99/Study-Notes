id: 684

Tag: DFS, 并查集

Date: 2021/1/13

Notes: Todo 并查集实现. 

C++实现

别忘了这个trivial的结论**树的边等于节点数量 - 1**

若存在，只会有一个环，所以用并查集的话，edges的遍历顺序是顺序即可

vector::clear() 

vector::resize(size)

vector::erase(vector::begin(), vector::end())

vector::remove(vector::begin(), vector::end(), val)

* vec.erase(vec.remove(vec.begin(), vec.end(), x), vec.end())

---

