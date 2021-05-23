Ordinal Data conversion#
As for differences in OrdinalEncoder and LabelEncoder implementation, the accepted answer mentions the shape of the data: (OrdinalEncoder for 2D data; shape (n_samples, n_features), LabelEncoder is for 1D data: for shape (n_samples,))

Maybe that's why the top-voted answer suggests OrdinalEncoder is for the "features" (often a 2D), whereas LabelEncoder is for the "target variable" (often a 1D array).

That's why a OrdinalEncoder would get an error:

ValueError: Expected 2D array, got 1D array instead: ...if trying to fit on 1D data: OrdinalEncoder().fit(['a','b'])

However, another difference between the encoders is the name of their learned parameter;

LabelEncoder learns classes_ OrdinalEncoder learns categories_ Notice the differences in fitting LabelEncoder vs OrdinalEncoder, and the differences in the values of the learned parameters.

LabelEncoder.fit(...) accepts a 1D array; LabelEncoder.classes_ is 1D OrdinalEncoder.fit(...) accepts a 2D array; OrdinalEncoder.categories_ is 2D. LabelEncoder().fit(['a','b']).classes_ # >>> array(['a', 'b'], dtype='<U1')

OrdinalEncoder().fit([['a'], ['b']]).categories_
# >>> [array(['a', 'b'], dtype=object)]
import pandas as pd
data= {"NO": [1,2,3,4,5,6,7] , 
       "Name": ["Tom","Harry","John","Nancy","Mike","Kate","Mary"],
       "Sex" : ["M","M","M","F","M","F","F"],
       "Blood" : ["O","A","A","B","O","AB","O"],
       "Study" : ["Math","Math","English","Biology","Math","English","Science"],
       
}
print(data)
{'NO': [1, 2, 3, 4, 5, 6, 7], 'Name': ['Tom', 'Harry', 'John', 'Nancy', 'Mike', 'Kate', 'Mary'], 'Sex': ['M', 'M', 'M', 'F', 'M', 'F', 'F'], 'Blood': ['O', 'A', 'A', 'B', 'O', 'AB', 'O'], 'Study': ['Math', 'Math', 'English', 'Biology', 'Math', 'English', 'Science']}
data1 = pd.DataFrame(data)
data1
NO	Name	Sex	Blood	Study
0	1	Tom	M	O	Math
1	2	Harry	M	A	Math
2	3	John	M	A	English
3	4	Nancy	F	B	Biology
4	5	Mike	M	O	Math
5	6	Kate	F	AB	English
6	7	Mary	F	O	Science
from sklearn.preprocessing import OrdinalEncoder
enco_ord = OrdinalEncoder()
data1[["Sex","Blood", "Study"]] = enco_ord.fit_transform(data1[["Sex","Blood", "Study"]])
data1
NO	Name	Sex	Blood	Study
0	1	Tom	1.0	3.0	2.0
1	2	Harry	1.0	0.0	2.0
2	3	John	1.0	0.0	1.0
3	4	Nancy	0.0	2.0	0.0
4	5	Mike	1.0	3.0	2.0
5	6	Kate	0.0	1.0	1.0
6	7	Mary	0.0	3.0	3.0
print(dir(enco_ord))
print(enco_ord.categories_)
['__class__', '__delattr__', '__dict__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__getstate__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__le__', '__lt__', '__module__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__setstate__', '__sizeof__', '__str__', '__subclasshook__', '__weakref__', '_check_X', '_check_n_features', '_fit', '_get_feature', '_get_param_names', '_get_tags', '_more_tags', '_repr_html_', '_repr_html_inner', '_repr_mimebundle_', '_transform', '_validate_data', 'categories', 'categories_', 'dtype', 'fit', 'fit_transform', 'get_params', 'inverse_transform', 'set_params', 'transform']
[array([0., 1.]), array([0., 1., 2., 3.]), array([0., 1., 2., 3.])]
