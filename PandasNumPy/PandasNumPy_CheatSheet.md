# Pandas و NumPy؛ چیت‌شیت مرور سریع

## NumPy: مفاهیم و متدهای اصلی

```python
import numpy as np

np.array([1, 2, 3])                 # ساخت آرایه
np.array([[1, 2], [3, 4]])          # آرایه دوبعدی
arr.shape                           # ابعاد آرایه
arr.dtype                           # نوع داده
arr.astype(int)                     # تبدیل نوع
arr.size                            # تعداد کل عناصر
arr.ndim                            # تعداد ابعاد
```

### ساخت آرایه‌های پرکاربرد

```python
np.zeros(3)                         # [0, 0, 0]
np.zeros((2, 2))                    # ماتریس 2×2 صفر
np.ones((3, 4))
np.eye(3)                           # ماتریس همانی
np.arange(0, 20, 2)                 # بازه با گام ثابت
np.linspace(16, 30, 5)              # 5 عدد با فاصله مساوی
np.random.seed(0)
np.random.rand(3, 3)                # اعداد تصادفی 0 تا 1
np.random.randint(0, 20, (3, 3))    # اعداد صحیح تصادفی
np.mean(arr)
np.sum(arr)
np.max(arr)
np.min(arr)
np.median(arr)
np.std(arr)
```

### ایندکس و اسلایس

```python
arr[0]                              # عنصر یا سطر اول
arr[0:10]                           # اسلایس، مانند لیست
arr_2d[1:]                          # حذف سطر اول
arr_2d[:2, 1:]                      # سطرهای 0 تا 1 و ستون‌های از 1 به بعد
arr_2d[1, 2]                        # عنصر سطر 1 ستون 2
```

اسلایس NumPy معمولاً **view** است و تغییر آن می‌تواند آرایه اصلی را هم تغییر دهد؛ برای کپی مستقل از `copy()` استفاده کنید.

```python
copy_arr = arr.copy()
arr.reshape(3, 4)                   # تغییر شکل بدون تغییر تعداد عنصر
np.concatenate([arr1, arr2], axis=0)
np.vstack([arr1, arr2])
np.hstack([arr1, arr2])
```

## Pandas: ساختارهای اصلی

```python
import pandas as pd

pd.Series([10, 20, 30], index=['a', 'b', 'c'])
pd.DataFrame({'name': ['A', 'B'], 'score': [10, 20]})
```

`Series` یک مجموعه یک‌بعدی و `DataFrame` یک جدول دو‌بعدی شامل سطر و ستون است.

## خواندن و نوشتن داده

```python
pd.read_csv('data.csv')
pd.read_csv('data.csv', index_col=0)
pd.read_csv('data.csv', parse_dates=['date'])
pd.read_excel('data.xlsx')
pd.read_excel('data.xlsx', sheet_name='Sheet1')
pd.read_html(url)                   # استخراج جدول‌های HTML

df.to_csv('new.csv', index=False)
df.to_excel('output.xlsx', sheet_name='Sheet1', index=False)
```

برای پایگاه SQLite:

```python
pd.read_sql('SELECT * FROM table_name', connection)
pd.read_sql_query('SELECT Country FROM populations', connection)
```

## مشاهده و خلاصه داده

```python
df.head()                           # چند سطر اول
df.tail()                           # چند سطر آخر
df.shape                            # (تعداد سطر، تعداد ستون)
df.info()                           # نوع‌ها و مقادیر ناموجود
df.describe()                       # آمار عددی
df.describe(include='all')          # آمار همه ستون‌ها
df.describe().transpose()
df.columns
df.dtypes
df.index
len(df)
df['column']
df[['col1', 'col2']]
df.sample(5)                        # نمونه تصادفی
```

## انتخاب سطر و ستون

```python
df.loc[2]                           # انتخاب برچسب سطر
df.loc[2, 'Age']
df.loc[[2, 5], ['Age', 'Sex']]

df.iloc[0]                          # اولین سطر
df.iloc[67]
df.iloc[:, 0:3]                     # ستون‌های موقعیتی
df.iloc[10:20]
```

قاعده: `loc` بر اساس نام/برچسب، و `iloc` بر اساس موقعیت عددی است.

## فیلتر و شرط‌ها

```python
df[df['total_bill'] > 30]
df[df['sex'] == 'Male']
df[(df['total_bill'] > 30) & (df['sex'] == 'Male')]
df[(df['day'] == 'Sat') | (df['day'] == 'Sun')]
df[df['day'].isin(['Sat', 'Sun'])]
df['sex'].notnull()
df['pre_movie_score'].isnull()
```

شرط‌های چندگانه را داخل پرانتز بگذارید؛ `&` یعنی AND و `|` یعنی OR.

## ایجاد، تغییر و حذف ستون

```python
df['new'] = df['a'] + df['b']
df['tip_percent'] = df['tip'] / df['total_bill'] * 100
df['total_stays'] = df['week_nights'] + df['weekend_nights']
df['quality'] = df.apply(
    lambda row: 'Generous' if row['tip'] / row['total_bill'] > 0.20 else 'Other',
    axis=1,
)

df.drop('column', axis=1)
df.drop(columns=['col1', 'col2'], inplace=True)
df.rename(columns={'old': 'new'})
df.set_index('column')
df.reset_index()
df.columns = ['a', 'b', 'c']
```

`inplace=True` تغییر را مستقیماً روی `df` اعمال می‌کند؛ در برخی کدها این گزینه deprecated است، پس معمولاً استفاده به این صورت امن‌تر است:

```python
df = df.drop(columns=['company'])
```

## مرتب‌سازی، نمونه و تعداد

```python
df.sort_values('tip')
df.sort_values(['tip', 'size'])
df.sort_values('mpg', ascending=False)
df.sort_index(ascending=False)
df['sex'].value_counts()
df['country'].value_counts().head(5)
df.nlargest(5, 'score')
df.nsmallest(5, 'score')
df.sample(5)
df.unique()
df['time'].unique()
```

## مقادیر خالی

```python
df.isnull()
df.notnull()
df.isnull().sum()
df.dropna()
df.dropna(axis=1)
df.dropna(subset=['country'])
df.fillna(0)
df.fillna(df.mean(numeric_only=True))
```

## توابع رشته‌ای

```python
s.str.capitalize()
s.str.lower()
s.str.upper()
s.str.strip()
s.str.replace(';', '')
s.str.split(',')
s.str.split(',', expand=True)
s.str[0]
s.str.isdigit()
df['name'].apply(lambda name: name.strip().title())
df['name'].apply(cleanup)
```

## توابع عددی و تجمیع

```python
df['a'].mean()
df['a'].median()
df['a'].sum()
df['a'].min()
df['a'].max()
df['a'].std()
df['a'].count()
df['a'].idxmax()
df['a'].idxmin()
df.corr()
df.agg({'mpg': 'mean'})
df.agg({'mpg': ['mean', 'median', 'max']})
df.agg(['sum', 'mean'])
df['a'].apply(lambda x: x ** 2)
np.vectorize(quality)(df['total_bill'], df['tip'])
```

## GroupBy

```python
df.groupby('model_year').mean(numeric_only=True)
df.groupby('model_year')['mpg'].mean()
df.groupby('model_year').count()
df.groupby('model_year').sum()
df.groupby('model_year').describe()['mpg']
df.groupby(['model_year', 'cylinders']).mean(numeric_only=True)

df.groupby('genre')['revenue'].sum().sort_values(ascending=False)
df.groupby('genre').agg({
    'quantity': ['sum', 'mean'],
    'revenue': ['sum', 'mean'],
})
```

برای MultiIndex:

```python
grouped.loc[70]
grouped.loc[(70, 6)]
grouped.xs(key=70, level='model_year')
grouped.xs(key=8, level='cylinders')
grouped.swaplevel()
grouped.sort_index(level='model_year', ascending=False)
```

## Pivot و تجمیع جدولی

```python
pd.pivot_table(
    df,
    index='Company',
    values=['Licenses', 'Sale Price'],
    columns='Product',
    aggfunc='sum',
    fill_value=0,
)

pd.pivot_table(
    df,
    index=['Account Manager', 'Contact'],
    values=['Sale Price', 'Licenses'],
    aggfunc='sum',
    margins=True,
)
```

نکته: اگر نیاز به تجمیع نیست، `pivot()` را در نظر بگیرید و برای تجمیع از `pivot_table()` یا `groupby()` استفاده کنید.

## اتصال جدول‌ها

```python
pd.concat([df1, df2], axis=0)       # زیر هم
pd.concat([df1, df2], axis=1)       # کنار هم
```

Joinها با `merge`:

```python
pd.merge(left, right, on='name', how='inner')
pd.merge(left, right, on='name', how='left')
pd.merge(left, right, on='name', how='right')
pd.merge(left, right, on='name', how='outer')

pd.merge(left, right, left_on='reg_name', right_on='name')
pd.merge(left, right, left_index=True, right_on='name')
pd.merge(left, right, on='name', suffixes=('_left', '_right'))
```

نوع Join: `inner` فقط مشترک‌ها، `left` همه سمت چپ، `right` همه سمت راست و `outer` همه دو طرف.

## تاریخ و زمان

```python
pd.to_datetime(s)
pd.to_datetime(s, dayfirst=True)
pd.to_datetime(s, format='%d--%b--%Y')
pd.to_datetime(s, format='mixed')

df['DATE'] = pd.to_datetime(df['DATE'])
df['DATE'].dt.year
df['DATE'].dt.month
df['DATE'].dt.day
df['DATE'].dt.day_name()
df['DATE'].dt.quarter

df.set_index('DATE').resample('YE').mean()
df.set_index('DATE').resample('ME').sum()
```

پس از `set_index('DATE')` می‌توان داده را با `resample` تجمیع بازه‌ای کرد؛ سپس برای بازگشت به ستون معمولی از `reset_index()` استفاده کنید.

## نکات مهم از تمرین‌ها

- بررسی اندازه داده: `len(df)`، `shape` و `count()`
- پیدا کردن ستون‌های خالی: `isnull().sum()`
- حذف ستون بلااستفاده با `drop(columns=...)`
- کشف بیشترین و کمترین: `idxmax()`، `idxmin()` و مقادیر متناظر با `loc`
- تکرارها و فراوانی گروه‌ها: `value_counts()`
- تبدیل مقادیر عددی متنی: `pd.to_numeric(errors='coerce')`
- استفاده از فیلتر برای تحلیل زیرمجموعه‌ها و سپس `groupby`
- محاسبه ستون مشتق‌شده، مثل مجموع اقامت یا کل مبلغ پرداخت‌شده
- بررسی همبستگی با `corr()`؛ همبستگی رابطه علیت را ثابت نمی‌کند

## ترتیب پیشنهادی مرور یک پروژه Pandas

```python
df = pd.read_csv('data.csv')

df.head()
df.info()
df.describe()
df.isnull().sum()

df = df.drop(columns=['unused_column'])
df['date'] = pd.to_datetime(df['date'], errors='coerce')
df = df.dropna(subset=['date'])
df = df.sort_values('date')

df.groupby('category')['value'].sum().sort_values(ascending=False)
pd.pivot_table(df, index='category', values='value', aggfunc='sum')
df.to_csv('clean_output.csv', index=False)
```
