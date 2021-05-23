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
