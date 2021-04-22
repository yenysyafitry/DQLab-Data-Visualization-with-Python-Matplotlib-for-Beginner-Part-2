### Membuat Multi-Line Chart 
<p align="justify">Kode untuk membaca dataset dan penambahan kolom baru dataset telah diberikan.Setelah mengetikkan baris-baris perintah dari baris ke-13 s/d 22 dan membuat grafik multi-line chart seperti berikut</p>

```plantuml
# Import library
import datetime
import pandas as pd
import matplotlib.pyplot as plt
# Baca dataset
dataset = pd.read_csv('https://storage.googleapis.com/dqlab-dataset/retail_raw_reduced.csv')
# Buat kolom baru yang bertipe datetime dalam format '%Y-%m'
dataset['order_month'] = dataset['order_date'].apply(lambda x: datetime.datetime.strptime(x, "%Y-%m-%d").strftime('%Y-%m'))
# Buat Kolom GMV
dataset['gmv'] = dataset['item_price']*dataset['quantity']

# Buat Multi-Line Chart
dataset.groupby(['order_month','brand'])['gmv'].sum().unstack().plot()
plt.title('Monthly GMV Year 2019 - Breakdown by Brand',loc='center',pad=30, fontsize=20, color='blue')
plt.xlabel('Order Month', fontsize = 15)
plt.ylabel('Total Amount (in Billions)',fontsize = 15)
plt.grid(color='darkgray', linestyle=':', linewidth=0.5)
plt.ylim(ymin=0)
labels, locations = plt.yticks()
plt.yticks(labels, (labels/1000000000).astype(int))
plt.gcf().set_size_inches(10, 5)
plt.tight_layout()
plt.show()
```

|Output : |
| :--     | 
| <img src="https://github.com/yenysyafitry/DQLab-Data-Visualization-with-Python-Matplotlib-for-Beginner-Part-2/blob/main/download.png">|
</br>
<a href="https://academy.dqlab.id/main/livecode/165/318/1483">Link materi : academy.dqlab.id/main/livecode/165/318/1483</a>

----

### Kustomisasi Legend 
Beberapa parameter yang bisa ditambahkan untuk legend:
<ol><li>
loc: untuk menentukan posisi legend, berikut beberapa lokasi legend yang bisa didefinisikan:</li>
<ul><li>'upper left', 'upper right', 'lower left', 'lower right':legend diletakkan di pojok dari axes (atas kiri, atas kanan, bawah kiri, atas kiri)</li>
<li>'upper center', 'lower center', 'center left', 'center right': legend diletakkan di tepi axes (atas tengah, bawah tengah, tengah kiri, tengah kanan)</li>
<li>'center': legend diletakkan di tengah-tengah axes</li>
<li>'best': matplotlib akan memilih satu dari sekian kemungkinan lokasi legend di atas yang paling tidak overlap dengan isi grafik</li></ul>
<li>bbox_to_anchor: biasanya digunakan untuk adjust lokasi dari legend. Bisa berisi 2 angka yang menunjukkan koordinat x dan y (misal (1.6,0.5) berarti geser 1.6 ke kanan dan 0.5 ke atas). Bisa juga berisi 4 angka, angka ketiga dan keempat menyatakan width (lebar) dan height (tinggi) dari legend.</li>
<li>shadow: jika diisi True, maka kotak legend akan memiliki bayangan.</li>
<li>ncol: jumlah kolom dari isi legend, defaultnya adalah 1</li>
<li>fontsize: ukuran huruf pada legend</li>
<li>title: memberikan judul pada legend</li>
	<li>title_fontsize: ukuran huruf pada judul legend</li></ol>

```plantuml
import matplotlib.pyplot as plt
dataset.groupby(['order_month','brand'])['gmv'].sum().unstack().plot()
plt.title('Monthly GMV Year 2019 - Breakdown by Brand',loc='center',pad=30, fontsize=20, color='blue')
plt.xlabel('Order Month', fontsize = 15)
plt.ylabel('Total Amount (in Billions)',fontsize = 15)
plt.grid(color='darkgray', linestyle=':', linewidth=0.5)
plt.ylim(ymin=0)
labels, locations = plt.yticks()
plt.yticks(labels, (labels/1000000000).astype(int))
plt.legend(loc='right', bbox_to_anchor=(1.6, 0.5), shadow=True, ncol=2)
plt.gcf().set_size_inches(12, 5)
plt.tight_layout()
plt.show()
```
|Output : |
| :--     | 
| <img src="https://github.com/yenysyafitry/DQLab-Data-Visualization-with-Python-Matplotlib-for-Beginner-Part-2/blob/main/download (1).png">|
</br>
<a href="https://academy.dqlab.id/main/livecode/165/318/1484">Link materi : academy.dqlab.id/main/livecode/165/318/1484</a>

----

### Kustomisasi Colormap 
```plantuml
import matplotlib.pyplot as plt
plt.clf()
dataset.groupby(['order_month','province'])['gmv'].sum().unstack().plot(cmap='Set1')
plt.title('Monthly GMV Year 2019 - Breakdown by Province', loc='center', pad=30, fontsize=20, color='blue')
plt.xlabel('Order Month', fontsize=15)
plt.ylabel('Total Amount (in Billions)', fontsize=15)
plt.grid(color='darkgray', linestyle=':', linewidth=0.5)
plt.ylim(ymin=0)
labels, locations = plt.yticks()
plt.yticks(labels, (labels/1000000000).astype(int))
plt.legend(loc='lower center', bbox_to_anchor=(0.5, -0.5), shadow=True, ncol=3,title='Province', fontsize=9, title_fontsize=11)
plt.gcf().set_size_inches(10,5)
plt.tight_layout()
plt.show()
```
|Output : |
| :--     | 
| <img src="https://github.com/yenysyafitry/DQLab-Data-Visualization-with-Python-Matplotlib-for-Beginner-Part-2/blob/main/download (2).png">|
</br>
<a href="https://academy.dqlab.id/main/livecode/165/318/1486">Link materi : academy.dqlab.id/main/livecode/165/318/1486</a>

----

### Membuat Line Chart GMV Breakdown by Top Provinces 
```plantuml
# Buat variabel untuk 5 propinsi dengan GMV tertinggi
top_provinces = (dataset.groupby('province')['gmv']
                        .sum()
                        .reset_index()
                        .sort_values(by='gmv', ascending=False)
                        .head(5))
print(top_provinces)

# Buat satu kolom lagi di dataset dengan nama province_top
dataset['province_top'] = dataset['province'].apply(lambda x: x if (x in top_provinces['province'].to_list()) else 'other')

# Plot multi-line chartnya
import matplotlib.pyplot as plt
dataset.groupby(['order_month','province_top'])['gmv'].sum().unstack().plot(marker='.', cmap='plasma')
plt.title('Monthly GMV Year 2019 - Breakdown by Province', loc='center', pad=30, fontsize=20, color='blue')
plt.xlabel('Order Month', fontsize=15)
plt.ylabel('Total Amount (in Billions)', fontsize=15)
plt.grid(color='darkgray', linestyle=':', linewidth=0.5)
plt.ylim(ymin=0)
labels, locations = plt.yticks()
plt.yticks(labels, (labels/1000000000).astype(int))
plt.legend(loc='upper center', bbox_to_anchor=(1.1, 1), shadow=True, ncol=1)
plt.gcf().set_size_inches(12,5)
plt.tight_layout()
plt.show()
```
|Output : |
| :--     | 
| Hello, World!|
</br>
<a href="https://academy.dqlab.id/main/livecode/165/318/1487">Link materi : academy.dqlab.id/main/livecode/165/318/1487</a>

----

### Membuat Anotasi 
```plantuml
import matplotlib.pyplot as plt
dataset.groupby(['order_month','province_top'])['gmv'].sum().unstack().plot(marker='.', cmap='plasma')
plt.title('Monthly GMV Year 2019 - Breakdown by Province',loc='center',pad=30, fontsize=20, color='blue')
plt.xlabel('Order Month', fontsize = 15)
plt.ylabel('Total Amount (in Billions)',fontsize = 15)
plt.grid(color='darkgray', linestyle=':', linewidth=0.5)
plt.ylim(ymin=0)
labels, locations = plt.yticks()
plt.yticks(labels, (labels/1000000000).astype(int))
plt.legend(loc='upper center', bbox_to_anchor=(1.1, 1), shadow=True, ncol=1)
# Anotasi pertama
plt.annotate('GMV other meningkat pesat', xy=(5, 900000000), 
			 xytext=(4, 1700000000), weight='bold', color='red',
			 arrowprops=dict(arrowstyle='fancy',
							 connectionstyle="arc3",
							 color='red'))

# Anotasi kedua
plt.annotate('DKI Jakarta mendominasi', xy=(3, 3350000000),
			 xytext=(0, 3700000000), weight='bold', color='red',
			 arrowprops=dict(arrowstyle='->',
							 connectionstyle="angle",
							 color='red'))

plt.gcf().set_size_inches(12, 5)
plt.tight_layout()
plt.show()
```
|Output : |
| :--     | 
| Hello, World!|
</br>
<a href="https://academy.dqlab.id/main/livecode/165/318/1489">Link materi : academy.dqlab.id/main/livecode/165/318/1489</a>

----

### Membuat Subset Data 
```plantuml
dataset_dki_q4 = dataset[(dataset['province']=='DKI Jakarta') & (dataset['order_month'] >= '2019-10')]
print(dataset_dki_q4.head())
```

|Output : |
| :--     | 
| Hello, World!|
</br>
<a href="https://academy.dqlab.id/main/livecode/165/319/1490">Link materi : academy.dqlab.id/main/livecode/165/319/1490</a>

----

### Membuat Pie Chart 
```plantuml
import matplotlib.pyplot as plt
gmv_per_city_dki_q4 = dataset_dki_q4.groupby('city')['gmv'].sum().reset_index()
plt.figure(figsize=(6,6))
plt.pie(gmv_per_city_dki_q4['gmv'], labels = gmv_per_city_dki_q4['city'], autopct='%1.2f%%')
plt.title('GMV Contribution Per City - DKI Jakarta in Q4 2019', loc='center', pad=30, fontsize=15, color='blue')
plt.show()
```
|Output : |
| :--     | 
| Hello, World!|
</br>
<a href="https://academy.dqlab.id/main/livecode/165/319/1491">Link materi : academy.dqlab.id/main/livecode/165/319/1491</a>

----

### Membuat Bar Chart 
```plantuml
import matplotlib.pyplot as plt
plt.clf()
dataset_dki_q4.groupby('city')['gmv'].sum().sort_values(ascending=False).plot(kind='bar', color='green')
plt.title('GMV Per City - DKI Jakarta in Q4 2019', loc='center', pad=30, fontsize=15, color='blue')
plt.xlabel('City', fontsize=15)
plt.ylabel('Total Amount (in Billions)', fontsize=15)
plt.ylim(ymin=0)
labels, locations = plt.yticks()
plt.yticks(labels, (labels/1000000000).astype(int))
plt.xticks(rotation=0)
plt.show()
```

|Output : |
| :--     | 
| Hello, World!|
</br>
<a href="https://academy.dqlab.id/main/livecode/165/319/1492">Link materi : academy.dqlab.id/main/livecode/165/319/1492</a>

----

### Membuat Multi-Bar Chart 
```plantuml
import matplotlib.pyplot as plt
dataset_dki_q4.groupby(['city', 'order_month'])['gmv'].sum().unstack().plot(kind='bar')
plt.title('GMV Per City, Breakdown by Month\nDKI Jakarta in Q4 2019', loc='center', pad=30, fontsize=15, color='blue')
plt.xlabel('Province', fontsize=12)
plt.ylabel('Total Amount (in Billions)', fontsize=12)
plt.legend(bbox_to_anchor=(1,1), shadow=True, title='Month')
plt.ylim(ymin=0)
labels, locations = plt.yticks()
plt.yticks(labels, (labels/1000000000).astype(int))
plt.xticks(rotation=45)
plt.tight_layout()
plt.show()
```
|Output : |
| :--     | 
| Hello, World!|
</br>
<a href="https://academy.dqlab.id/main/livecode/165/319/1493">Link materi : academy.dqlab.id/main/livecode/165/319/1493</a>

----

### Membuat Stacked Chart 
```plantuml
import matplotlib.pyplot as plt
dataset_dki_q4.groupby(['order_month','city'])['gmv'].sum().sort_values(ascending=False).unstack().plot(kind='bar', stacked=True)
plt.title('GMV Per Month, Breakdown by City\nDKI Jakarta in Q4 2019', loc='center', pad=30, fontsize=15, color='blue')
plt.xlabel('Order Month', fontsize=12)
plt.ylabel('Total Amount (in Billions)', fontsize=12)
plt.legend(bbox_to_anchor=(1,1), shadow=True, ncol=1, title='City')
plt.ylim(ymin=0)
labels, locations = plt.yticks()
plt.yticks(labels, (labels/1000000000).astype(int))
plt.xticks(rotation=0)
plt.tight_layout()
plt.show()
```
|Output : |
| :--     | 
| Hello, World!|
</br>
<a href="https://academy.dqlab.id/main/livecode/165/319/1494">Link materi : academy.dqlab.id/main/livecode/165/319/1494</a>

----

### Membuat Agregat Data Customer 
```plantuml
data_per_customer = (dataset_dki_q4.groupby('customer_id')
                                   .agg({'order_id':'nunique', 
                                         'quantity': 'sum', 
                                         'gmv':'sum'})
                                   .reset_index()
                                   .rename(columns={'order_id':'orders'}))
print(data_per_customer.sort_values(by='orders', ascending=False))
```
|Output : |
| :--     | 
| Hello, World!|
</br>
<a href="https://academy.dqlab.id/main/livecode/165/320/1496">Link materi : academy.dqlab.id/main/livecode/165/320/1496</a>

----

### Membuat Histogram - Part 1 
```plantuml
import matplotlib.pyplot as plt
plt.clf()
# Histogram pertama
plt.figure()
plt.hist(data_per_customer['orders'])
plt.show()
# Histogram kedua
plt.figure()
plt.hist(data_per_customer['orders'], range=(1,5))
plt.title('Distribution of Number of Orders per Customer\nDKI Jakarta in Q4 2019', fontsize=15, color='blue')
plt.xlabel('Number of Orders', fontsize=12)
plt.ylabel('Number of Customers', fontsize=12)
plt.show()
```

|Output : |
| :--     | 
| Hello, World!|
</br>
<a href="https://academy.dqlab.id/main/livecode/165/320/1497">Link materi : academy.dqlab.id/main/livecode/165/320/1497</a>

----

### Membuat Histogram - Part 2 
```plantuml
import matplotlib.pyplot as plt
plt.figure(figsize=(10,5))
plt.hist(data_per_customer['quantity'], bins=100, range=(1,200), color='brown')
plt.title('Distribution of Total Quantity per Customer\nDKI Jakartain Q4 2019', fontsize=15, color='blue')
plt.xlabel('Quantity', fontsize=12)
plt.ylabel('Number of Customers', fontsize=12)
plt.xlim(xmin=0, xmax=200)
plt.show()
```

|Output : |
| :--     | 
| Hello, World!|
</br>
<a href="https://academy.dqlab.id/main/livecode/165/320/1498">Link materi : academy.dqlab.id/main/livecode/165/320/1498</a>

----

### Membuat Histogram - Part 3 
```plantuml
import matplotlib.pyplot as plt
plt.figure(figsize=(10,5))
plt.hist(data_per_customer['gmv'], bins=100, range=(1,200000000), color='green')
plt.title('Distribution of Total GMV per Customer\nDKI Jakartain Q4 2019', fontsize=15, color='blue')
plt.xlabel('GMV (in Millions)', fontsize=12)
plt.ylabel('Number of Customers', fontsize=12)
plt.xlim(xmin=0, xmax=200000000)
labels, locations = plt.xticks()
plt.xticks(labels, (labels/1000000).astype(int))
plt.show()
```
|Output : |
| :--     | 
| Hello, World!|
</br>
<a href="https://academy.dqlab.id/main/livecode/165/320/1499">Link materi : academy.dqlab.id/main/livecode/165/320/1499</a>

----

### Membuat Scatterplot 
```plantuml
import matplotlib.pyplot as plt
plt.clf()
# Scatterplot pertama
plt.figure()
plt.scatter(data_per_customer['quantity'], data_per_customer['gmv'])
plt.show()
# Scatterplot kedua: perbaikan scatterplot pertama
plt.figure(figsize=(10,8))
plt.scatter(data_per_customer['quantity'], data_per_customer['gmv'], marker='+', color='red')
plt.title('Correlation of Quantity and GMV per Customer\nDKI Jakartain Q4 2019', fontsize=15, color='blue')
plt.xlabel('Quantity', fontsize=12)
plt.ylabel('GMV (in Millions)', fontsize=12)
plt.xlim(xmin=0, xmax=300)
plt.ylim(ymin=0, ymax=150000000)
labels, locations = plt.yticks()
plt.yticks(labels, (labels/1000000).astype(int))
plt.show()
```
|Output : |
| :--     | 
| Hello, World!|
</br>
<a href="https://academy.dqlab.id/main/livecode/165/320/1502">Link materi : academy.dqlab.id/main/livecode/165/320/1502</a>

----

### Case 1: Menentukan brand top 5 
```plantuml
#mengambil informasi top 5 brands berdasarkan quantity
top_brands = (dataset[dataset['order_month']=='2019-12'].groupby('brand')['quantity']
                .sum()
                .reset_index()
                .sort_values(by='quantity',ascending=False)
                .head(5))

#membuat dataframe baru, filter hanya di bulan Desember 2019 dan hanya top 5 brands
dataset_top5brand_dec = dataset[(dataset['order_month']=='2019-12') & (dataset['brand'].isin(top_brands['brand'].to_list()))]

# print top brands
print(top_brands)
```
|Output : |
| :--     | 
| Hello, World!|
</br>
<a href="https://academy.dqlab.id/main/livecode/165/321/1504">Link materi : academy.dqlab.id/main/livecode/165/321/1504</a>

----

### Case 2: Multi-line chart daily quantity untuk brand top 5 
```plantuml
import matplotlib.pyplot as plt
dataset_top5brand_dec.groupby(['order_date','brand'])['quantity'].sum().unstack().plot(marker='.', cmap='plasma')
plt.title('Daily Sold Quantity Dec 2019 - Breakdown by Brands',loc='center',pad=30, fontsize=15, color='blue')
plt.xlabel('Order Date', fontsize = 12)
plt.ylabel('Quantity',fontsize = 12)
plt.grid(color='darkgray', linestyle=':', linewidth=0.5)
plt.ylim(ymin=0)
plt.legend(loc='upper center', bbox_to_anchor=(1.1, 1), shadow=True, ncol=1)
plt.annotate('Terjadi lonjakan', xy=(7, 310), xytext=(8, 300),
weight='bold', color='red',
arrowprops=dict(arrowstyle='->',
connectionstyle="arc3",
color='red'))
plt.gcf().set_size_inches(10, 5)
plt.tight_layout()
plt.show()
```
|Output : |
| :--     | 
| Hello, World!|
</br>
<a href="https://academy.dqlab.id/main/livecode/165/321/1505">Link materi : academy.dqlab.id/main/livecode/165/321/1505</a>

----

### Case 3: Kuantitas penjualan brand top 5 selama Desember 2019 
```plantuml
import matplotlib.pyplot as plt
plt.clf()
dataset_top5brand_dec.groupby('brand')['product_id'].nunique().sort_values(ascending=False).plot(kind='bar', color='green')
plt.title('Number of Sold Products per Brand, December 2019',loc='center',pad=30, fontsize=15, color='blue')
plt.xlabel('Brand', fontsize = 15)
plt.ylabel('Number of Products',fontsize = 15)
plt.ylim(ymin=0)
plt.xticks(rotation=0)
plt.show()
```
|Output : |
| :--     | 
| Hello, World!|
</br>
<a href="https://academy.dqlab.id/main/livecode/165/321/1506">Link materi : academy.dqlab.id/main/livecode/165/321/1506</a>

----

### Case 4: Penjulan produk diatas 100 dan dibawah 100 selama Desember 2019 
```plantuml
import matplotlib.pyplot as plt
#membuat dataframe baru, untuk agregat jumlah quantity terjual per product
dataset_top5brand_dec_per_product = dataset_top5brand_dec.groupby(['brand','product_id'])['quantity'].sum().reset_index()

#beri kolom baru untuk menandai product yang terjual >= 100 dan <100
dataset_top5brand_dec_per_product['quantity_group'] = dataset_top5brand_dec_per_product['quantity'].apply(lambda x: '>= 100' if x>=100 else '< 100')
dataset_top5brand_dec_per_product.sort_values('quantity',ascending=False,inplace=True)

#membuat referensi pengurutan brand berdasarkan banyaknya semua product
s_sort = dataset_top5brand_dec_per_product.groupby('brand')['product_id'].nunique().sort_values(ascending=False)

#plot stacked barchart
dataset_top5brand_dec_per_product.groupby(['brand','quantity_group'])['product_id'].nunique().reindex(index=s_sort.index, level='brand').unstack().plot(kind='bar', stacked=True)
plt.title('Number of Sold Products per Brand, December 2019',loc='center',pad=30, fontsize=15, color='blue')
plt.xlabel('Brand', fontsize = 15)
plt.ylabel('Number of Products',fontsize = 15)
plt.ylim(ymin=0)
plt.xticks(rotation=0)
plt.show()
```
|Output : |
| :--     | 
| Hello, World!|
</br>
<a href="https://academy.dqlab.id/main/livecode/165/321/1507">Link materi : academy.dqlab.id/main/livecode/165/321/1507</a>

----

### Case 5: Murah atau mahalkah harga produk brand top 5 
```plantuml
import matplotlib.pyplot as plt
plt.figure(figsize=(10,5))
plt.hist(dataset_top5brand_dec.groupby('product_id')['item_price'].median(), bins=10, stacked=True, range=(1,2000000), color='green')
plt.title('Distribution of Price Median per Product\nTop 5 Brands in Dec 2019',fontsize=15, color='blue')
plt.xlabel('Price Median', fontsize = 12)
plt.ylabel('Number of Products',fontsize = 12)
plt.xlim(xmin=0,xmax=2000000)
plt.show()
```
|Output : |
| :--     | 
| Hello, World!|
</br>
<a href="https://academy.dqlab.id/main/livecode/165/321/1508">Link materi : academy.dqlab.id/main/livecode/165/321/1508</a>

----

### Case 6a: Korelasi quantity vs GMV 
```plantuml
import matplotlib.pyplot as plt
#agregat per product
data_per_product_top5brand_dec = dataset_top5brand_dec.groupby('product_id').agg({'quantity': 'sum', 'gmv':'sum', 'item_price':'median'}).reset_index()
#scatter plot
plt.figure(figsize=(10,8))
plt.scatter(data_per_product_top5brand_dec['quantity'],data_per_product_top5brand_dec['gmv'], marker='+', color='red')
plt.title('Correlation of Quantity and GMV per Product\nTop 5 Brands in December 2019',fontsize=15, color='blue')
plt.xlabel('Quantity', fontsize = 12)
plt.ylabel('GMV (in Millions)',fontsize = 12)
plt.xlim(xmin=0,xmax=300)
plt.ylim(ymin=0,ymax=200000000)
labels, locations = plt.yticks()
plt.yticks(labels, (labels/1000000).astype(int))
plt.show()
```
|Output : |
| :--     | 
| Hello, World!|
</br>
<a href="https://academy.dqlab.id/main/livecode/165/321/1509">Link materi : academy.dqlab.id/main/livecode/165/321/1509</a>

----

### Case 6b: Korelasi median harga vs quantity 

```plantuml
import matplotlib.pyplot as plt
plt.clf()
#agregat per product
data_per_product_top5brand_dec = dataset_top5brand_dec.groupby('product_id').agg({'quantity': 'sum', 'gmv':'sum', 'item_price':'median'}).reset_index()
#scatter plot
plt.figure(figsize=(10,8))
plt.scatter(data_per_product_top5brand_dec['item_price'],data_per_product_top5brand_dec['quantity'], marker='o', color='green')
plt.title('Correlation of Quantity and GMV per Product\nTop 5 Brands in December 2019',fontsize=15, color='blue')
plt.xlabel('Price Median', fontsize = 12)
plt.ylabel('Quantity',fontsize = 12)
plt.xlim(xmin=0,xmax=2000000)
plt.ylim(ymin=0,ymax=250)
plt.show()
```
|Output : |
| :--     | 
| Hello, World!|
</br>
<a href="https://academy.dqlab.id/main/livecode/165/321/240">Link materi : academy.dqlab.id/main/livecode/165/321/240</a>

----
