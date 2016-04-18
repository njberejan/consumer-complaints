Please launch the Jupyter Notebook file to view the results from this assignment. 

Within the notebook, you will find code which compiles the data from the provided data file into a Data Frame object.
Next, you will see code which converts the date received column to an index for use in determining the day of the week the 
complaint was issued (which is used to find the mean # of complaints by day of the week.)

Next, you will come across culling of data from the data frame into usable sets, and the accompanying graphs 
(in both bar and pie form!) which demonstrate the number of complaints by product, company, and company response. 
The final set of data and graphs as of this writing are supposed to show the mean number of complaints by day of the week;
however, I am not certain that this is what the numbers displayed are actually reflecting. It seems like it should be higher.

Thanks and enjoy!


In [122]:
import matplotlib.pyplot as plt
import numpy as np
import pandas as pd
from pandas import DataFrame, Series
import seaborn
%matplotlib inline
Importing the Data
In [123]:
consumer_complaints = pd.read_csv('complaints_dec_2014.csv')
A sample of the data:
In [124]:
consumer_complaints.head()
Out[124]:
Complaint ID	Product	Sub-product	Issue	Sub-issue	State	ZIP code	Submitted via	Date received	Date sent to company	Company	Company response	Timely response?	Consumer disputed?
0	1177167	Debt collection	NaN	Cont'd attempts collect debt not owed	Debt is not mine	TX	77068.0	Web	12/31/2014	12/31/2014	Ad Astra Recovery Services Inc	Closed with explanation	Yes	NaN
1	1177166	Debt collection	NaN	Cont'd attempts collect debt not owed	Debt is not mine	TX	77068.0	Web	12/31/2014	12/31/2014	Unique Management Services, Inc	Closed with explanation	Yes	NaN
2	1177165	Debt collection	NaN	Cont'd attempts collect debt not owed	Debt is not mine	TX	77068.0	Web	12/31/2014	12/31/2014	CL Holdings, LLC	Closed with monetary relief	Yes	NaN
3	1177164	Debt collection	NaN	Cont'd attempts collect debt not owed	Debt is not mine	TX	77068.0	Web	12/31/2014	12/31/2014	Enhanced Recovery Company, LLC	Closed with non-monetary relief	Yes	NaN
4	1177163	Debt collection	NaN	Cont'd attempts collect debt not owed	Debt is not mine	TX	77068.0	Web	12/31/2014	12/31/2014	Enhanced Acquisitions, LLC	Closed with explanation	Yes	NaN
Resolving Date and Timestamp Issues
In [125]:
date_series = consumer_complaints.pop('Date received')
In [126]:
consumer_complaints.index = pd.to_datetime(date_series, format='%m/%d/%Y')
In [127]:
consumer_complaints['Weekday'] = consumer_complaints.index.weekday
Number of Complaints by Company
In [128]:
complaint_and_company = consumer_complaints[['Complaint ID', 'Company']]
                    
In [129]:
sorted_complaint_and_company = complaint_and_company.sort_values(by='Company')
In [130]:
popped_companies = sorted_complaint_and_company.pop('Company')
In [131]:
graph_values = popped_companies.value_counts().head(10)
In [132]:
graph_values
Out[132]:
Bank of America        766
Equifax                737
Experian               675
TransUnion             604
Wells Fargo            598
JPMorgan Chase         545
Ocwen                  408
Citibank               403
Nationstar Mortgage    357
Capital One            252
Name: Company, dtype: int64
In [133]:
labels = 'Bank of America', 'Equifax', 'Experian', 'TransUnion', 'Wells Fargo', 'JPMorgan Chase', 'Ocwen', 'Citibank', 'Nationstar Mortgage', 'Capital One'
sizes = [766, 737, 675, 604, 598, 545, 408, 403, 357, 252]
colors = ['yellowgreen', 'gold', 'lightskyblue', 'lightcoral', 'red', 'green', 'blue', 'orange', 'purple', 'aqua']

plt.pie(sizes, labels=labels, colors=colors,
        autopct='%1.1f%%', shadow=True, startangle=90)
plt.axis('equal')
fig = plt.gcf()
fig.set_size_inches(8,8)
plt.show()

In [134]:
graph_values.plot.bar()
Out[134]:
<matplotlib.axes._subplots.AxesSubplot at 0x10ccde4a8>

Number of Complaints by Product
In [135]:
complaint_and_product = consumer_complaints[['Complaint ID', 'Product']]
In [136]:
sorted_complaint_and_product = complaint_and_product.sort_values(by='Product')
In [137]:
popped_products = sorted_complaint_and_product.pop('Product')
In [138]:
values_for_graph = popped_products.value_counts()
In [139]:
values_for_graph
Out[139]:
Mortgage                   3002
Debt collection            2942
Credit reporting           2113
Bank account or service    1136
Credit card                1100
Consumer loan               578
Student loan                340
Payday loan                 141
Money transfers             107
Prepaid card                 70
Other financial service      14
Name: Product, dtype: int64
In [140]:
labels = 'Mortgage', 'Debt collection', 'Credit reporting', 'Bank account or service', 'Credit card', 'Consumer loan', 'Student loan', 'Payday loan', 'Money transfers', 'Prepaid card', 'Other'
sizes = [3002, 2942, 2113, 1136, 1100, 578, 340, 141, 107, 70, 14]
colors = ['green', 'gold']

plt.pie(sizes, labels=labels, colors=colors,
        autopct='%1.1f%%', shadow=True, startangle=180)
plt.axis('equal')
fig = plt.gcf()
fig.set_size_inches(8,8)
plt.show()

In [141]:
values_for_graph.plot.barh()
Out[141]:
<matplotlib.axes._subplots.AxesSubplot at 0x10d41b048>

Number of Complaints by Company Response
In [142]:
complaint_and_response = consumer_complaints[['Complaint ID', 'Company response']]
In [143]:
sorted_complaint_and_response = complaint_and_response.sort_values(by='Company response')
In [144]:
popped_response = sorted_complaint_and_response.pop('Company response')
In [145]:
g_values = popped_response.value_counts()
In [146]:
g_values
Out[146]:
Closed with explanation            8185
Closed with non-monetary relief    1253
In progress                        1056
Closed with monetary relief         643
Closed                              239
Untimely response                   167
Name: Company response, dtype: int64
In [147]:
labels = 'Closed with explanation', 'Closed with non-monetary relief', 'In progress', 'Closed with monetary relief', 'Closed', 'Untimely Response'
sizes = [8185, 1253, 1056, 643, 239, 167]
colors = ['white', 'red']

plt.pie(sizes, labels=labels, colors=colors,
        autopct='%1.1f%%', shadow=True, startangle=180)
plt.axis('equal')
fig = plt.gcf()
fig.set_size_inches(8,8)
plt.show()

In [148]:
g_values.plot.bar()
Out[148]:
<matplotlib.axes._subplots.AxesSubplot at 0x10d970d30>

Mean Number of Complaints by Day of the Week
In [149]:
complaint_and_day = consumer_complaints[['Complaint ID', 'Weekday']]
In [150]:
sorted_complaint_and_day = complaint_and_day.sort_values(by='Weekday')
In [151]:
popped_weekday = sorted_complaint_and_day.pop('Weekday')
In [156]:
graph_vs = popped_weekday.value_counts('mean')
In [157]:
graph_vs
Out[157]:
1    0.223512
0    0.220393
2    0.198302
3    0.141558
4    0.125271
5    0.047041
6    0.043923
Name: Weekday, dtype: float64
In [158]:
labels = '1', '0', '2', '3', '4', '5', '6'
sizes = [0.223512, 0.220393, 0.198302, 0.141558, 0.125271, 0.047041, 0.043923]
colors = ['gold', 'blue',]

plt.pie(sizes, labels=labels, colors=colors,
        autopct='%1.1f%%', shadow=True, startangle=180)
plt.axis('equal')
fig = plt.gcf()
fig.set_size_inches(8,8)
plt.show()

In [159]:
graph_vs.plot.barh()
Out[159]:
<matplotlib.axes._subplots.AxesSubplot at 0x103d0d470>
