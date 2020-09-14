---
categories:
- ""
- ""
date: "2020-09-04"
description: Climate change and temperature anomalies 
draft: false
image: pic11.jpg
keywords: ""
slug: blog1
title: Climate Change
output:
  pdf_document:
    toc: yes
  html_document:
    theme: flatly
    highlight: zenburn
    number_sections: yes
    toc: yes
    toc_float: yes
    code_folding: show
---











# Climate change and temperature anomalies 


To study climate change, we have used data on the *Combined Land-Surface Air and Sea-Surface Water Temperature Anomalies* in the Northern Hemisphere from [NASA's Goddard Institute for Space Studies](https://data.giss.nasa.gov/gistemp).

The base period to calculate changes is taken as 1951-1980.While loading the data, `skip` and `na` are used to remove the first row and missing values respectively:


```r
weather <- 
  read_csv("https://data.giss.nasa.gov/gistemp/tabledata_v3/NH.Ts+dSST.csv", 
           skip = 1, 
           na = "***")
```


We've converted the dataframe from wide to 'long' format and defined variables month as `month`, and the temperature deviation values as `delta`.


```r
glimpse(weather)
```

```
## Rows: 140
## Columns: 19
## $ Year  <dbl> 1880, 1881, 1882, 1883, 1884, 1885, 1886, 1887, 1888, 1889, 1...
## $ Jan   <dbl> -0.54, -0.19, 0.22, -0.59, -0.23, -1.00, -0.68, -1.07, -0.53,...
## $ Feb   <dbl> -0.38, -0.25, 0.22, -0.67, -0.11, -0.37, -0.68, -0.58, -0.59,...
## $ Mar   <dbl> -0.26, 0.02, 0.00, -0.16, -0.65, -0.21, -0.57, -0.36, -0.58, ...
## $ Apr   <dbl> -0.37, -0.02, -0.36, -0.27, -0.62, -0.53, -0.34, -0.42, -0.24...
## $ May   <dbl> -0.11, -0.06, -0.32, -0.32, -0.42, -0.55, -0.34, -0.27, -0.16...
## $ Jun   <dbl> -0.22, -0.36, -0.38, -0.26, -0.52, -0.47, -0.43, -0.20, -0.04...
## $ Jul   <dbl> -0.23, -0.06, -0.37, -0.09, -0.48, -0.39, -0.20, -0.23, 0.04,...
## $ Aug   <dbl> -0.24, -0.03, -0.14, -0.26, -0.50, -0.44, -0.47, -0.52, -0.19...
## $ Sep   <dbl> -0.26, -0.23, -0.17, -0.33, -0.45, -0.32, -0.34, -0.17, -0.12...
## $ Oct   <dbl> -0.32, -0.40, -0.53, -0.21, -0.41, -0.30, -0.31, -0.40, 0.04,...
## $ Nov   <dbl> -0.37, -0.42, -0.32, -0.40, -0.48, -0.28, -0.45, -0.19, -0.03...
## $ Dec   <dbl> -0.48, -0.28, -0.42, -0.25, -0.40, 0.00, -0.17, -0.43, -0.26,...
## $ `J-D` <dbl> -0.32, -0.19, -0.21, -0.32, -0.44, -0.40, -0.42, -0.40, -0.22...
## $ `D-N` <dbl> NA, -0.21, -0.20, -0.33, -0.43, -0.44, -0.40, -0.38, -0.24, -...
## $ DJF   <dbl> NA, -0.31, 0.06, -0.56, -0.20, -0.59, -0.45, -0.61, -0.52, -0...
## $ MAM   <dbl> -0.24, -0.02, -0.22, -0.25, -0.56, -0.43, -0.42, -0.35, -0.33...
## $ JJA   <dbl> -0.23, -0.15, -0.30, -0.20, -0.50, -0.44, -0.37, -0.32, -0.06...
## $ SON   <dbl> -0.32, -0.35, -0.34, -0.32, -0.44, -0.30, -0.37, -0.25, -0.04...
```

```r
## convert df from wide to long, creating a "Month" and a "Delta" variable 

tidyweather <- weather %>% select(1:13) %>% pivot_longer(cols = 2:13, names_to = "month", values_to = "delta")
tidyweather %>% kbl() %>% kable_styling()
```

\begin{table}[H]
\centering
\begin{tabular}[t]{r|l|r}
\hline
Year & month & delta\\
\hline
1880 & Jan & -0.54\\
\hline
1880 & Feb & -0.38\\
\hline
1880 & Mar & -0.26\\
\hline
1880 & Apr & -0.37\\
\hline
1880 & May & -0.11\\
\hline
1880 & Jun & -0.22\\
\hline
1880 & Jul & -0.23\\
\hline
1880 & Aug & -0.24\\
\hline
1880 & Sep & -0.26\\
\hline
1880 & Oct & -0.32\\
\hline
1880 & Nov & -0.37\\
\hline
1880 & Dec & -0.48\\
\hline
1881 & Jan & -0.19\\
\hline
1881 & Feb & -0.25\\
\hline
1881 & Mar & 0.02\\
\hline
1881 & Apr & -0.02\\
\hline
1881 & May & -0.06\\
\hline
1881 & Jun & -0.36\\
\hline
1881 & Jul & -0.06\\
\hline
1881 & Aug & -0.03\\
\hline
1881 & Sep & -0.23\\
\hline
1881 & Oct & -0.40\\
\hline
1881 & Nov & -0.42\\
\hline
1881 & Dec & -0.28\\
\hline
1882 & Jan & 0.22\\
\hline
1882 & Feb & 0.22\\
\hline
1882 & Mar & 0.00\\
\hline
1882 & Apr & -0.36\\
\hline
1882 & May & -0.32\\
\hline
1882 & Jun & -0.38\\
\hline
1882 & Jul & -0.37\\
\hline
1882 & Aug & -0.14\\
\hline
1882 & Sep & -0.17\\
\hline
1882 & Oct & -0.53\\
\hline
1882 & Nov & -0.32\\
\hline
1882 & Dec & -0.42\\
\hline
1883 & Jan & -0.59\\
\hline
1883 & Feb & -0.67\\
\hline
1883 & Mar & -0.16\\
\hline
1883 & Apr & -0.27\\
\hline
1883 & May & -0.32\\
\hline
1883 & Jun & -0.26\\
\hline
1883 & Jul & -0.09\\
\hline
1883 & Aug & -0.26\\
\hline
1883 & Sep & -0.33\\
\hline
1883 & Oct & -0.21\\
\hline
1883 & Nov & -0.40\\
\hline
1883 & Dec & -0.25\\
\hline
1884 & Jan & -0.23\\
\hline
1884 & Feb & -0.11\\
\hline
1884 & Mar & -0.65\\
\hline
1884 & Apr & -0.62\\
\hline
1884 & May & -0.42\\
\hline
1884 & Jun & -0.52\\
\hline
1884 & Jul & -0.48\\
\hline
1884 & Aug & -0.50\\
\hline
1884 & Sep & -0.45\\
\hline
1884 & Oct & -0.41\\
\hline
1884 & Nov & -0.48\\
\hline
1884 & Dec & -0.40\\
\hline
1885 & Jan & -1.00\\
\hline
1885 & Feb & -0.37\\
\hline
1885 & Mar & -0.21\\
\hline
1885 & Apr & -0.53\\
\hline
1885 & May & -0.55\\
\hline
1885 & Jun & -0.47\\
\hline
1885 & Jul & -0.39\\
\hline
1885 & Aug & -0.44\\
\hline
1885 & Sep & -0.32\\
\hline
1885 & Oct & -0.30\\
\hline
1885 & Nov & -0.28\\
\hline
1885 & Dec & 0.00\\
\hline
1886 & Jan & -0.68\\
\hline
1886 & Feb & -0.68\\
\hline
1886 & Mar & -0.57\\
\hline
1886 & Apr & -0.34\\
\hline
1886 & May & -0.34\\
\hline
1886 & Jun & -0.43\\
\hline
1886 & Jul & -0.20\\
\hline
1886 & Aug & -0.47\\
\hline
1886 & Sep & -0.34\\
\hline
1886 & Oct & -0.31\\
\hline
1886 & Nov & -0.45\\
\hline
1886 & Dec & -0.17\\
\hline
1887 & Jan & -1.07\\
\hline
1887 & Feb & -0.58\\
\hline
1887 & Mar & -0.36\\
\hline
1887 & Apr & -0.42\\
\hline
1887 & May & -0.27\\
\hline
1887 & Jun & -0.20\\
\hline
1887 & Jul & -0.23\\
\hline
1887 & Aug & -0.52\\
\hline
1887 & Sep & -0.17\\
\hline
1887 & Oct & -0.40\\
\hline
1887 & Nov & -0.19\\
\hline
1887 & Dec & -0.43\\
\hline
1888 & Jan & -0.53\\
\hline
1888 & Feb & -0.59\\
\hline
1888 & Mar & -0.58\\
\hline
1888 & Apr & -0.24\\
\hline
1888 & May & -0.16\\
\hline
1888 & Jun & -0.04\\
\hline
1888 & Jul & 0.04\\
\hline
1888 & Aug & -0.19\\
\hline
1888 & Sep & -0.12\\
\hline
1888 & Oct & 0.04\\
\hline
1888 & Nov & -0.03\\
\hline
1888 & Dec & -0.26\\
\hline
1889 & Jan & -0.31\\
\hline
1889 & Feb & 0.35\\
\hline
1889 & Mar & 0.07\\
\hline
1889 & Apr & 0.15\\
\hline
1889 & May & -0.05\\
\hline
1889 & Jun & -0.12\\
\hline
1889 & Jul & -0.10\\
\hline
1889 & Aug & -0.16\\
\hline
1889 & Sep & -0.26\\
\hline
1889 & Oct & -0.34\\
\hline
1889 & Nov & -0.61\\
\hline
1889 & Dec & -0.55\\
\hline
1890 & Jan & -0.65\\
\hline
1890 & Feb & -0.56\\
\hline
1890 & Mar & -0.44\\
\hline
1890 & Apr & -0.36\\
\hline
1890 & May & -0.48\\
\hline
1890 & Jun & -0.20\\
\hline
1890 & Jul & -0.24\\
\hline
1890 & Aug & -0.38\\
\hline
1890 & Sep & -0.40\\
\hline
1890 & Oct & -0.07\\
\hline
1890 & Nov & -0.69\\
\hline
1890 & Dec & -0.43\\
\hline
1891 & Jan & -0.59\\
\hline
1891 & Feb & -0.68\\
\hline
1891 & Mar & -0.12\\
\hline
1891 & Apr & -0.32\\
\hline
1891 & May & -0.21\\
\hline
1891 & Jun & -0.14\\
\hline
1891 & Jul & -0.12\\
\hline
1891 & Aug & -0.09\\
\hline
1891 & Sep & -0.03\\
\hline
1891 & Oct & -0.26\\
\hline
1891 & Nov & -0.56\\
\hline
1891 & Dec & -0.02\\
\hline
1892 & Jan & -0.45\\
\hline
1892 & Feb & -0.16\\
\hline
1892 & Mar & -0.56\\
\hline
1892 & Apr & -0.48\\
\hline
1892 & May & -0.28\\
\hline
1892 & Jun & -0.11\\
\hline
1892 & Jul & -0.38\\
\hline
1892 & Aug & -0.28\\
\hline
1892 & Sep & -0.13\\
\hline
1892 & Oct & -0.22\\
\hline
1892 & Nov & -0.81\\
\hline
1892 & Dec & -0.65\\
\hline
1893 & Jan & -1.47\\
\hline
1893 & Feb & -0.89\\
\hline
1893 & Mar & -0.28\\
\hline
1893 & Apr & -0.30\\
\hline
1893 & May & -0.44\\
\hline
1893 & Jun & -0.22\\
\hline
1893 & Jul & -0.13\\
\hline
1893 & Aug & -0.33\\
\hline
1893 & Sep & -0.22\\
\hline
1893 & Oct & -0.23\\
\hline
1893 & Nov & -0.26\\
\hline
1893 & Dec & -0.63\\
\hline
1894 & Jan & -0.87\\
\hline
1894 & Feb & -0.43\\
\hline
1894 & Mar & -0.21\\
\hline
1894 & Apr & -0.59\\
\hline
1894 & May & -0.33\\
\hline
1894 & Jun & -0.48\\
\hline
1894 & Jul & -0.24\\
\hline
1894 & Aug & -0.22\\
\hline
1894 & Sep & -0.26\\
\hline
1894 & Oct & -0.24\\
\hline
1894 & Nov & -0.47\\
\hline
1894 & Dec & -0.31\\
\hline
1895 & Jan & -0.73\\
\hline
1895 & Feb & -0.77\\
\hline
1895 & Mar & -0.39\\
\hline
1895 & Apr & -0.23\\
\hline
1895 & May & -0.30\\
\hline
1895 & Jun & -0.26\\
\hline
1895 & Jul & -0.18\\
\hline
1895 & Aug & -0.24\\
\hline
1895 & Sep & 0.02\\
\hline
1895 & Oct & -0.04\\
\hline
1895 & Nov & -0.06\\
\hline
1895 & Dec & -0.24\\
\hline
1896 & Jan & -0.44\\
\hline
1896 & Feb & -0.21\\
\hline
1896 & Mar & -0.49\\
\hline
1896 & Apr & -0.60\\
\hline
1896 & May & -0.22\\
\hline
1896 & Jun & 0.01\\
\hline
1896 & Jul & -0.02\\
\hline
1896 & Aug & -0.07\\
\hline
1896 & Sep & -0.05\\
\hline
1896 & Oct & 0.14\\
\hline
1896 & Nov & -0.25\\
\hline
1896 & Dec & -0.18\\
\hline
1897 & Jan & -0.38\\
\hline
1897 & Feb & -0.34\\
\hline
1897 & Mar & -0.39\\
\hline
1897 & Apr & -0.05\\
\hline
1897 & May & 0.07\\
\hline
1897 & Jun & -0.13\\
\hline
1897 & Jul & -0.01\\
\hline
1897 & Aug & -0.09\\
\hline
1897 & Sep & -0.03\\
\hline
1897 & Oct & -0.13\\
\hline
1897 & Nov & -0.25\\
\hline
1897 & Dec & -0.28\\
\hline
1898 & Jan & 0.03\\
\hline
1898 & Feb & -0.45\\
\hline
1898 & Mar & -0.77\\
\hline
1898 & Apr & -0.30\\
\hline
1898 & May & -0.31\\
\hline
1898 & Jun & -0.11\\
\hline
1898 & Jul & -0.14\\
\hline
1898 & Aug & -0.16\\
\hline
1898 & Sep & -0.17\\
\hline
1898 & Oct & -0.35\\
\hline
1898 & Nov & -0.46\\
\hline
1898 & Dec & -0.26\\
\hline
1899 & Jan & -0.13\\
\hline
1899 & Feb & -0.67\\
\hline
1899 & Mar & -0.53\\
\hline
1899 & Apr & -0.20\\
\hline
1899 & May & -0.26\\
\hline
1899 & Jun & -0.38\\
\hline
1899 & Jul & -0.20\\
\hline
1899 & Aug & -0.03\\
\hline
1899 & Sep & 0.00\\
\hline
1899 & Oct & 0.07\\
\hline
1899 & Nov & 0.38\\
\hline
1899 & Dec & -0.48\\
\hline
1900 & Jan & -0.60\\
\hline
1900 & Feb & -0.06\\
\hline
1900 & Mar & 0.12\\
\hline
1900 & Apr & -0.14\\
\hline
1900 & May & -0.03\\
\hline
1900 & Jun & -0.15\\
\hline
1900 & Jul & -0.13\\
\hline
1900 & Aug & -0.07\\
\hline
1900 & Sep & 0.02\\
\hline
1900 & Oct & 0.25\\
\hline
1900 & Nov & -0.15\\
\hline
1900 & Dec & -0.01\\
\hline
1901 & Jan & -0.35\\
\hline
1901 & Feb & 0.07\\
\hline
1901 & Mar & 0.35\\
\hline
1901 & Apr & 0.11\\
\hline
1901 & May & -0.12\\
\hline
1901 & Jun & 0.03\\
\hline
1901 & Jul & 0.02\\
\hline
1901 & Aug & -0.13\\
\hline
1901 & Sep & -0.26\\
\hline
1901 & Oct & -0.31\\
\hline
1901 & Nov & -0.19\\
\hline
1901 & Dec & -0.40\\
\hline
1902 & Jan & -0.15\\
\hline
1902 & Feb & 0.13\\
\hline
1902 & Mar & -0.37\\
\hline
1902 & Apr & -0.39\\
\hline
1902 & May & -0.43\\
\hline
1902 & Jun & -0.43\\
\hline
1902 & Jul & -0.32\\
\hline
1902 & Aug & -0.29\\
\hline
1902 & Sep & -0.32\\
\hline
1902 & Oct & -0.29\\
\hline
1902 & Nov & -0.49\\
\hline
1902 & Dec & -0.64\\
\hline
1903 & Jan & -0.22\\
\hline
1903 & Feb & 0.20\\
\hline
1903 & Mar & -0.14\\
\hline
1903 & Apr & -0.45\\
\hline
1903 & May & -0.48\\
\hline
1903 & Jun & -0.51\\
\hline
1903 & Jul & -0.45\\
\hline
1903 & Aug & -0.59\\
\hline
1903 & Sep & -0.47\\
\hline
1903 & Oct & -0.47\\
\hline
1903 & Nov & -0.43\\
\hline
1903 & Dec & -0.55\\
\hline
1904 & Jan & -0.82\\
\hline
1904 & Feb & -0.56\\
\hline
1904 & Mar & -0.40\\
\hline
1904 & Apr & -0.57\\
\hline
1904 & May & -0.49\\
\hline
1904 & Jun & -0.51\\
\hline
1904 & Jul & -0.60\\
\hline
1904 & Aug & -0.52\\
\hline
1904 & Sep & -0.57\\
\hline
1904 & Oct & -0.37\\
\hline
1904 & Nov & -0.11\\
\hline
1904 & Dec & -0.34\\
\hline
1905 & Jan & -0.50\\
\hline
1905 & Feb & -0.79\\
\hline
1905 & Mar & -0.24\\
\hline
1905 & Apr & -0.46\\
\hline
1905 & May & -0.32\\
\hline
1905 & Jun & -0.32\\
\hline
1905 & Jul & -0.23\\
\hline
1905 & Aug & -0.23\\
\hline
1905 & Sep & -0.14\\
\hline
1905 & Oct & -0.24\\
\hline
1905 & Nov & 0.07\\
\hline
1905 & Dec & -0.25\\
\hline
1906 & Jan & -0.55\\
\hline
1906 & Feb & -0.57\\
\hline
1906 & Mar & -0.17\\
\hline
1906 & Apr & 0.06\\
\hline
1906 & May & -0.23\\
\hline
1906 & Jun & -0.10\\
\hline
1906 & Jul & -0.28\\
\hline
1906 & Aug & -0.12\\
\hline
1906 & Sep & -0.19\\
\hline
1906 & Oct & -0.13\\
\hline
1906 & Nov & -0.39\\
\hline
1906 & Dec & 0.02\\
\hline
1907 & Jan & -0.54\\
\hline
1907 & Feb & -0.71\\
\hline
1907 & Mar & -0.27\\
\hline
1907 & Apr & -0.47\\
\hline
1907 & May & -0.60\\
\hline
1907 & Jun & -0.51\\
\hline
1907 & Jul & -0.41\\
\hline
1907 & Aug & -0.36\\
\hline
1907 & Sep & -0.36\\
\hline
1907 & Oct & -0.19\\
\hline
1907 & Nov & -0.59\\
\hline
1907 & Dec & -0.58\\
\hline
1908 & Jan & -0.52\\
\hline
1908 & Feb & -0.24\\
\hline
1908 & Mar & -0.67\\
\hline
1908 & Apr & -0.48\\
\hline
1908 & May & -0.36\\
\hline
1908 & Jun & -0.36\\
\hline
1908 & Jul & -0.40\\
\hline
1908 & Aug & -0.55\\
\hline
1908 & Sep & -0.29\\
\hline
1908 & Oct & -0.44\\
\hline
1908 & Nov & -0.58\\
\hline
1908 & Dec & -0.53\\
\hline
1909 & Jan & -0.86\\
\hline
1909 & Feb & -0.46\\
\hline
1909 & Mar & -0.61\\
\hline
1909 & Apr & -0.68\\
\hline
1909 & May & -0.51\\
\hline
1909 & Jun & -0.51\\
\hline
1909 & Jul & -0.42\\
\hline
1909 & Aug & -0.30\\
\hline
1909 & Sep & -0.31\\
\hline
1909 & Oct & -0.28\\
\hline
1909 & Nov & -0.09\\
\hline
1909 & Dec & -0.54\\
\hline
1910 & Jan & -0.39\\
\hline
1910 & Feb & -0.41\\
\hline
1910 & Mar & -0.48\\
\hline
1910 & Apr & -0.36\\
\hline
1910 & May & -0.32\\
\hline
1910 & Jun & -0.44\\
\hline
1910 & Jul & -0.39\\
\hline
1910 & Aug & -0.41\\
\hline
1910 & Sep & -0.37\\
\hline
1910 & Oct & -0.26\\
\hline
1910 & Nov & -0.55\\
\hline
1910 & Dec & -0.78\\
\hline
1911 & Jan & -0.73\\
\hline
1911 & Feb & -0.58\\
\hline
1911 & Mar & -0.60\\
\hline
1911 & Apr & -0.43\\
\hline
1911 & May & -0.47\\
\hline
1911 & Jun & -0.32\\
\hline
1911 & Jul & -0.37\\
\hline
1911 & Aug & -0.40\\
\hline
1911 & Sep & -0.31\\
\hline
1911 & Oct & -0.11\\
\hline
1911 & Nov & -0.12\\
\hline
1911 & Dec & -0.29\\
\hline
1912 & Jan & -0.35\\
\hline
1912 & Feb & -0.11\\
\hline
1912 & Mar & -0.55\\
\hline
1912 & Apr & -0.22\\
\hline
1912 & May & -0.15\\
\hline
1912 & Jun & -0.18\\
\hline
1912 & Jul & -0.55\\
\hline
1912 & Aug & -0.76\\
\hline
1912 & Sep & -0.70\\
\hline
1912 & Oct & -0.81\\
\hline
1912 & Nov & -0.48\\
\hline
1912 & Dec & -0.56\\
\hline
1913 & Jan & -0.57\\
\hline
1913 & Feb & -0.58\\
\hline
1913 & Mar & -0.44\\
\hline
1913 & Apr & -0.49\\
\hline
1913 & May & -0.61\\
\hline
1913 & Jun & -0.56\\
\hline
1913 & Jul & -0.56\\
\hline
1913 & Aug & -0.47\\
\hline
1913 & Sep & -0.43\\
\hline
1913 & Oct & -0.41\\
\hline
1913 & Nov & -0.12\\
\hline
1913 & Dec & 0.10\\
\hline
1914 & Jan & 0.18\\
\hline
1914 & Feb & -0.10\\
\hline
1914 & Mar & -0.30\\
\hline
1914 & Apr & -0.36\\
\hline
1914 & May & -0.23\\
\hline
1914 & Jun & -0.26\\
\hline
1914 & Jul & -0.36\\
\hline
1914 & Aug & -0.23\\
\hline
1914 & Sep & -0.24\\
\hline
1914 & Oct & 0.00\\
\hline
1914 & Nov & -0.26\\
\hline
1914 & Dec & -0.12\\
\hline
1915 & Jan & -0.18\\
\hline
1915 & Feb & -0.02\\
\hline
1915 & Mar & -0.22\\
\hline
1915 & Apr & 0.06\\
\hline
1915 & May & -0.04\\
\hline
1915 & Jun & -0.09\\
\hline
1915 & Jul & -0.06\\
\hline
1915 & Aug & -0.13\\
\hline
1915 & Sep & -0.13\\
\hline
1915 & Oct & -0.27\\
\hline
1915 & Nov & -0.01\\
\hline
1915 & Dec & -0.15\\
\hline
1916 & Jan & 0.00\\
\hline
1916 & Feb & -0.10\\
\hline
1916 & Mar & -0.38\\
\hline
1916 & Apr & -0.33\\
\hline
1916 & May & -0.32\\
\hline
1916 & Jun & -0.43\\
\hline
1916 & Jul & -0.30\\
\hline
1916 & Aug & -0.31\\
\hline
1916 & Sep & -0.41\\
\hline
1916 & Oct & -0.25\\
\hline
1916 & Nov & -0.36\\
\hline
1916 & Dec & -1.10\\
\hline
1917 & Jan & -0.56\\
\hline
1917 & Feb & -0.63\\
\hline
1917 & Mar & -0.68\\
\hline
1917 & Apr & -0.56\\
\hline
1917 & May & -0.74\\
\hline
1917 & Jun & -0.48\\
\hline
1917 & Jul & -0.31\\
\hline
1917 & Aug & -0.39\\
\hline
1917 & Sep & -0.23\\
\hline
1917 & Oct & -0.57\\
\hline
1917 & Nov & -0.21\\
\hline
1917 & Dec & -1.08\\
\hline
1918 & Jan & -0.57\\
\hline
1918 & Feb & -0.39\\
\hline
1918 & Mar & -0.23\\
\hline
1918 & Apr & -0.62\\
\hline
1918 & May & -0.53\\
\hline
1918 & Jun & -0.43\\
\hline
1918 & Jul & -0.37\\
\hline
1918 & Aug & -0.32\\
\hline
1918 & Sep & -0.21\\
\hline
1918 & Oct & -0.02\\
\hline
1918 & Nov & -0.20\\
\hline
1918 & Dec & -0.52\\
\hline
1919 & Jan & -0.37\\
\hline
1919 & Feb & -0.38\\
\hline
1919 & Mar & -0.36\\
\hline
1919 & Apr & -0.25\\
\hline
1919 & May & -0.44\\
\hline
1919 & Jun & -0.41\\
\hline
1919 & Jul & -0.34\\
\hline
1919 & Aug & -0.29\\
\hline
1919 & Sep & -0.14\\
\hline
1919 & Oct & -0.07\\
\hline
1919 & Nov & -0.47\\
\hline
1919 & Dec & -0.54\\
\hline
1920 & Jan & -0.09\\
\hline
1920 & Feb & -0.19\\
\hline
1920 & Mar & 0.08\\
\hline
1920 & Apr & -0.24\\
\hline
1920 & May & -0.21\\
\hline
1920 & Jun & -0.25\\
\hline
1920 & Jul & -0.23\\
\hline
1920 & Aug & -0.27\\
\hline
1920 & Sep & -0.30\\
\hline
1920 & Oct & -0.38\\
\hline
1920 & Nov & -0.43\\
\hline
1920 & Dec & -0.65\\
\hline
1921 & Jan & 0.26\\
\hline
1921 & Feb & -0.09\\
\hline
1921 & Mar & -0.07\\
\hline
1921 & Apr & -0.14\\
\hline
1921 & May & -0.22\\
\hline
1921 & Jun & -0.12\\
\hline
1921 & Jul & -0.01\\
\hline
1921 & Aug & -0.23\\
\hline
1921 & Sep & -0.12\\
\hline
1921 & Oct & 0.03\\
\hline
1921 & Nov & -0.07\\
\hline
1921 & Dec & -0.11\\
\hline
1922 & Jan & -0.37\\
\hline
1922 & Feb & -0.62\\
\hline
1922 & Mar & -0.06\\
\hline
1922 & Apr & -0.17\\
\hline
1922 & May & -0.20\\
\hline
1922 & Jun & -0.16\\
\hline
1922 & Jul & -0.24\\
\hline
1922 & Aug & -0.31\\
\hline
1922 & Sep & -0.35\\
\hline
1922 & Oct & -0.29\\
\hline
1922 & Nov & -0.10\\
\hline
1922 & Dec & -0.12\\
\hline
1923 & Jan & -0.21\\
\hline
1923 & Feb & -0.42\\
\hline
1923 & Mar & -0.40\\
\hline
1923 & Apr & -0.52\\
\hline
1923 & May & -0.40\\
\hline
1923 & Jun & -0.19\\
\hline
1923 & Jul & -0.24\\
\hline
1923 & Aug & -0.21\\
\hline
1923 & Sep & -0.12\\
\hline
1923 & Oct & 0.13\\
\hline
1923 & Nov & 0.23\\
\hline
1923 & Dec & 0.12\\
\hline
1924 & Jan & -0.26\\
\hline
1924 & Feb & -0.21\\
\hline
1924 & Mar & 0.06\\
\hline
1924 & Apr & -0.27\\
\hline
1924 & May & -0.06\\
\hline
1924 & Jun & -0.15\\
\hline
1924 & Jul & -0.15\\
\hline
1924 & Aug & -0.21\\
\hline
1924 & Sep & -0.18\\
\hline
1924 & Oct & -0.20\\
\hline
1924 & Nov & 0.14\\
\hline
1924 & Dec & -0.35\\
\hline
1925 & Jan & -0.24\\
\hline
1925 & Feb & -0.35\\
\hline
1925 & Mar & -0.19\\
\hline
1925 & Apr & -0.14\\
\hline
1925 & May & -0.23\\
\hline
1925 & Jun & -0.24\\
\hline
1925 & Jul & -0.24\\
\hline
1925 & Aug & -0.12\\
\hline
1925 & Sep & 0.02\\
\hline
1925 & Oct & -0.06\\
\hline
1925 & Nov & 0.22\\
\hline
1925 & Dec & 0.28\\
\hline
1926 & Jan & 0.52\\
\hline
1926 & Feb & 0.26\\
\hline
1926 & Mar & 0.37\\
\hline
1926 & Apr & -0.11\\
\hline
1926 & May & -0.21\\
\hline
1926 & Jun & -0.16\\
\hline
1926 & Jul & -0.15\\
\hline
1926 & Aug & -0.07\\
\hline
1926 & Sep & -0.02\\
\hline
1926 & Oct & 0.08\\
\hline
1926 & Nov & 0.18\\
\hline
1926 & Dec & -0.27\\
\hline
1927 & Jan & -0.27\\
\hline
1927 & Feb & -0.08\\
\hline
1927 & Mar & -0.44\\
\hline
1927 & Apr & -0.28\\
\hline
1927 & May & -0.14\\
\hline
1927 & Jun & -0.11\\
\hline
1927 & Jul & -0.02\\
\hline
1927 & Aug & -0.09\\
\hline
1927 & Sep & 0.07\\
\hline
1927 & Oct & 0.27\\
\hline
1927 & Nov & 0.15\\
\hline
1927 & Dec & -0.37\\
\hline
1928 & Jan & 0.29\\
\hline
1928 & Feb & 0.12\\
\hline
1928 & Mar & -0.27\\
\hline
1928 & Apr & -0.31\\
\hline
1928 & May & -0.15\\
\hline
1928 & Jun & -0.29\\
\hline
1928 & Jul & -0.09\\
\hline
1928 & Aug & -0.16\\
\hline
1928 & Sep & -0.10\\
\hline
1928 & Oct & -0.05\\
\hline
1928 & Nov & 0.06\\
\hline
1928 & Dec & -0.07\\
\hline
1929 & Jan & -0.55\\
\hline
1929 & Feb & -0.81\\
\hline
1929 & Mar & -0.17\\
\hline
1929 & Apr & -0.32\\
\hline
1929 & May & -0.28\\
\hline
1929 & Jun & -0.35\\
\hline
1929 & Jul & -0.30\\
\hline
1929 & Aug & -0.14\\
\hline
1929 & Sep & -0.16\\
\hline
1929 & Oct & 0.04\\
\hline
1929 & Nov & -0.01\\
\hline
1929 & Dec & -0.64\\
\hline
1930 & Jan & -0.11\\
\hline
1930 & Feb & -0.19\\
\hline
1930 & Mar & 0.16\\
\hline
1930 & Apr & -0.15\\
\hline
1930 & May & -0.19\\
\hline
1930 & Jun & -0.11\\
\hline
1930 & Jul & 0.01\\
\hline
1930 & Aug & 0.09\\
\hline
1930 & Sep & 0.03\\
\hline
1930 & Oct & 0.10\\
\hline
1930 & Nov & 0.41\\
\hline
1930 & Dec & 0.07\\
\hline
1931 & Jan & 0.05\\
\hline
1931 & Feb & -0.29\\
\hline
1931 & Mar & 0.00\\
\hline
1931 & Apr & -0.15\\
\hline
1931 & May & -0.13\\
\hline
1931 & Jun & 0.17\\
\hline
1931 & Jul & 0.11\\
\hline
1931 & Aug & 0.11\\
\hline
1931 & Sep & 0.19\\
\hline
1931 & Oct & 0.31\\
\hline
1931 & Nov & 0.15\\
\hline
1931 & Dec & 0.03\\
\hline
1932 & Jan & 0.39\\
\hline
1932 & Feb & -0.21\\
\hline
1932 & Mar & -0.22\\
\hline
1932 & Apr & 0.10\\
\hline
1932 & May & -0.12\\
\hline
1932 & Jun & -0.14\\
\hline
1932 & Jul & -0.13\\
\hline
1932 & Aug & -0.09\\
\hline
1932 & Sep & 0.09\\
\hline
1932 & Oct & 0.13\\
\hline
1932 & Nov & -0.29\\
\hline
1932 & Dec & -0.18\\
\hline
1933 & Jan & -0.36\\
\hline
1933 & Feb & -0.37\\
\hline
1933 & Mar & -0.30\\
\hline
1933 & Apr & -0.19\\
\hline
1933 & May & -0.26\\
\hline
1933 & Jun & -0.29\\
\hline
1933 & Jul & -0.12\\
\hline
1933 & Aug & -0.15\\
\hline
1933 & Sep & -0.23\\
\hline
1933 & Oct & -0.11\\
\hline
1933 & Nov & -0.22\\
\hline
1933 & Dec & -0.56\\
\hline
1934 & Jan & -0.21\\
\hline
1934 & Feb & 0.32\\
\hline
1934 & Mar & -0.33\\
\hline
1934 & Apr & -0.27\\
\hline
1934 & May & 0.05\\
\hline
1934 & Jun & -0.03\\
\hline
1934 & Jul & -0.02\\
\hline
1934 & Aug & -0.08\\
\hline
1934 & Sep & -0.13\\
\hline
1934 & Oct & 0.11\\
\hline
1934 & Nov & 0.28\\
\hline
1934 & Dec & 0.17\\
\hline
1935 & Jan & -0.39\\
\hline
1935 & Feb & 0.60\\
\hline
1935 & Mar & 0.06\\
\hline
1935 & Apr & -0.32\\
\hline
1935 & May & -0.32\\
\hline
1935 & Jun & -0.14\\
\hline
1935 & Jul & -0.05\\
\hline
1935 & Aug & -0.06\\
\hline
1935 & Sep & -0.03\\
\hline
1935 & Oct & 0.10\\
\hline
1935 & Nov & -0.34\\
\hline
1935 & Dec & -0.20\\
\hline
1936 & Jan & -0.29\\
\hline
1936 & Feb & -0.50\\
\hline
1936 & Mar & -0.13\\
\hline
1936 & Apr & -0.05\\
\hline
1936 & May & -0.02\\
\hline
1936 & Jun & 0.06\\
\hline
1936 & Jul & 0.13\\
\hline
1936 & Aug & 0.04\\
\hline
1936 & Sep & 0.07\\
\hline
1936 & Oct & 0.05\\
\hline
1936 & Nov & 0.06\\
\hline
1936 & Dec & 0.13\\
\hline
1937 & Jan & -0.02\\
\hline
1937 & Feb & 0.28\\
\hline
1937 & Mar & -0.15\\
\hline
1937 & Apr & -0.07\\
\hline
1937 & May & 0.08\\
\hline
1937 & Jun & 0.14\\
\hline
1937 & Jul & 0.14\\
\hline
1937 & Aug & 0.16\\
\hline
1937 & Sep & 0.34\\
\hline
1937 & Oct & 0.33\\
\hline
1937 & Nov & 0.27\\
\hline
1937 & Dec & -0.07\\
\hline
1938 & Jan & 0.24\\
\hline
1938 & Feb & 0.19\\
\hline
1938 & Mar & 0.32\\
\hline
1938 & Apr & 0.29\\
\hline
1938 & May & 0.01\\
\hline
1938 & Jun & -0.09\\
\hline
1938 & Jul & -0.03\\
\hline
1938 & Aug & 0.09\\
\hline
1938 & Sep & 0.19\\
\hline
1938 & Oct & 0.26\\
\hline
1938 & Nov & 0.25\\
\hline
1938 & Dec & -0.30\\
\hline
1939 & Jan & -0.01\\
\hline
1939 & Feb & -0.02\\
\hline
1939 & Mar & -0.25\\
\hline
1939 & Apr & -0.06\\
\hline
1939 & May & -0.02\\
\hline
1939 & Jun & -0.01\\
\hline
1939 & Jul & 0.03\\
\hline
1939 & Aug & 0.07\\
\hline
1939 & Sep & 0.14\\
\hline
1939 & Oct & 0.08\\
\hline
1939 & Nov & 0.23\\
\hline
1939 & Dec & 0.82\\
\hline
1940 & Jan & -0.20\\
\hline
1940 & Feb & 0.24\\
\hline
1940 & Mar & 0.17\\
\hline
1940 & Apr & 0.31\\
\hline
1940 & May & 0.13\\
\hline
1940 & Jun & 0.09\\
\hline
1940 & Jul & 0.15\\
\hline
1940 & Aug & 0.03\\
\hline
1940 & Sep & 0.22\\
\hline
1940 & Oct & 0.23\\
\hline
1940 & Nov & 0.32\\
\hline
1940 & Dec & 0.31\\
\hline
1941 & Jan & 0.26\\
\hline
1941 & Feb & 0.52\\
\hline
1941 & Mar & 0.10\\
\hline
1941 & Apr & 0.16\\
\hline
1941 & May & 0.19\\
\hline
1941 & Jun & 0.18\\
\hline
1941 & Jul & 0.24\\
\hline
1941 & Aug & 0.18\\
\hline
1941 & Sep & 0.11\\
\hline
1941 & Oct & 0.47\\
\hline
1941 & Nov & 0.21\\
\hline
1941 & Dec & 0.18\\
\hline
1942 & Jan & 0.32\\
\hline
1942 & Feb & -0.08\\
\hline
1942 & Mar & 0.09\\
\hline
1942 & Apr & 0.11\\
\hline
1942 & May & 0.12\\
\hline
1942 & Jun & 0.10\\
\hline
1942 & Jul & 0.00\\
\hline
1942 & Aug & -0.10\\
\hline
1942 & Sep & 0.06\\
\hline
1942 & Oct & 0.23\\
\hline
1942 & Nov & 0.23\\
\hline
1942 & Dec & 0.16\\
\hline
1943 & Jan & -0.15\\
\hline
1943 & Feb & 0.30\\
\hline
1943 & Mar & -0.08\\
\hline
1943 & Apr & 0.23\\
\hline
1943 & May & 0.18\\
\hline
1943 & Jun & -0.10\\
\hline
1943 & Jul & -0.03\\
\hline
1943 & Aug & 0.10\\
\hline
1943 & Sep & 0.14\\
\hline
1943 & Oct & 0.44\\
\hline
1943 & Nov & 0.36\\
\hline
1943 & Dec & 0.39\\
\hline
1944 & Jan & 0.60\\
\hline
1944 & Feb & 0.46\\
\hline
1944 & Mar & 0.32\\
\hline
1944 & Apr & 0.18\\
\hline
1944 & May & 0.23\\
\hline
1944 & Jun & 0.18\\
\hline
1944 & Jul & 0.14\\
\hline
1944 & Aug & 0.14\\
\hline
1944 & Sep & 0.37\\
\hline
1944 & Oct & 0.43\\
\hline
1944 & Nov & 0.19\\
\hline
1944 & Dec & -0.03\\
\hline
1945 & Jan & 0.10\\
\hline
1945 & Feb & -0.10\\
\hline
1945 & Mar & 0.10\\
\hline
1945 & Apr & 0.26\\
\hline
1945 & May & 0.06\\
\hline
1945 & Jun & 0.07\\
\hline
1945 & Jul & 0.04\\
\hline
1945 & Aug & 0.30\\
\hline
1945 & Sep & 0.20\\
\hline
1945 & Oct & 0.21\\
\hline
1945 & Nov & 0.09\\
\hline
1945 & Dec & -0.25\\
\hline
1946 & Jan & 0.35\\
\hline
1946 & Feb & 0.20\\
\hline
1946 & Mar & 0.10\\
\hline
1946 & Apr & 0.31\\
\hline
1946 & May & 0.02\\
\hline
1946 & Jun & -0.03\\
\hline
1946 & Jul & -0.04\\
\hline
1946 & Aug & -0.06\\
\hline
1946 & Sep & -0.03\\
\hline
1946 & Oct & 0.03\\
\hline
1946 & Nov & 0.08\\
\hline
1946 & Dec & -0.53\\
\hline
1947 & Jan & -0.14\\
\hline
1947 & Feb & -0.02\\
\hline
1947 & Mar & 0.31\\
\hline
1947 & Apr & 0.24\\
\hline
1947 & May & 0.02\\
\hline
1947 & Jun & 0.01\\
\hline
1947 & Jul & -0.05\\
\hline
1947 & Aug & -0.07\\
\hline
1947 & Sep & -0.03\\
\hline
1947 & Oct & 0.37\\
\hline
1947 & Nov & 0.17\\
\hline
1947 & Dec & -0.09\\
\hline
1948 & Jan & 0.32\\
\hline
1948 & Feb & -0.16\\
\hline
1948 & Mar & -0.29\\
\hline
1948 & Apr & -0.02\\
\hline
1948 & May & 0.18\\
\hline
1948 & Jun & 0.06\\
\hline
1948 & Jul & 0.01\\
\hline
1948 & Aug & 0.02\\
\hline
1948 & Sep & -0.01\\
\hline
1948 & Oct & 0.04\\
\hline
1948 & Nov & 0.07\\
\hline
1948 & Dec & -0.36\\
\hline
1949 & Jan & 0.29\\
\hline
1949 & Feb & -0.14\\
\hline
1949 & Mar & 0.03\\
\hline
1949 & Apr & -0.02\\
\hline
1949 & May & 0.01\\
\hline
1949 & Jun & -0.17\\
\hline
1949 & Jul & -0.12\\
\hline
1949 & Aug & -0.04\\
\hline
1949 & Sep & -0.09\\
\hline
1949 & Oct & 0.08\\
\hline
1949 & Nov & 0.04\\
\hline
1949 & Dec & -0.22\\
\hline
1950 & Jan & -0.42\\
\hline
1950 & Feb & -0.30\\
\hline
1950 & Mar & 0.07\\
\hline
1950 & Apr & -0.22\\
\hline
1950 & May & -0.03\\
\hline
1950 & Jun & -0.06\\
\hline
1950 & Jul & -0.21\\
\hline
1950 & Aug & -0.23\\
\hline
1950 & Sep & -0.10\\
\hline
1950 & Oct & -0.09\\
\hline
1950 & Nov & -0.39\\
\hline
1950 & Dec & -0.09\\
\hline
1951 & Jan & -0.36\\
\hline
1951 & Feb & -0.54\\
\hline
1951 & Mar & -0.18\\
\hline
1951 & Apr & 0.07\\
\hline
1951 & May & 0.17\\
\hline
1951 & Jun & 0.00\\
\hline
1951 & Jul & 0.09\\
\hline
1951 & Aug & 0.21\\
\hline
1951 & Sep & 0.32\\
\hline
1951 & Oct & 0.26\\
\hline
1951 & Nov & 0.11\\
\hline
1951 & Dec & 0.34\\
\hline
1952 & Jan & 0.17\\
\hline
1952 & Feb & 0.15\\
\hline
1952 & Mar & -0.25\\
\hline
1952 & Apr & 0.15\\
\hline
1952 & May & 0.15\\
\hline
1952 & Jun & 0.15\\
\hline
1952 & Jul & 0.13\\
\hline
1952 & Aug & 0.04\\
\hline
1952 & Sep & 0.14\\
\hline
1952 & Oct & -0.02\\
\hline
1952 & Nov & -0.23\\
\hline
1952 & Dec & 0.03\\
\hline
1953 & Jan & 0.25\\
\hline
1953 & Feb & 0.36\\
\hline
1953 & Mar & 0.26\\
\hline
1953 & Apr & 0.42\\
\hline
1953 & May & 0.23\\
\hline
1953 & Jun & 0.22\\
\hline
1953 & Jul & 0.21\\
\hline
1953 & Aug & 0.17\\
\hline
1953 & Sep & 0.16\\
\hline
1953 & Oct & 0.25\\
\hline
1953 & Nov & 0.03\\
\hline
1953 & Dec & 0.18\\
\hline
1954 & Jan & -0.30\\
\hline
1954 & Feb & -0.05\\
\hline
1954 & Mar & -0.17\\
\hline
1954 & Apr & -0.07\\
\hline
1954 & May & -0.11\\
\hline
1954 & Jun & -0.04\\
\hline
1954 & Jul & -0.11\\
\hline
1954 & Aug & 0.00\\
\hline
1954 & Sep & 0.08\\
\hline
1954 & Oct & 0.08\\
\hline
1954 & Nov & 0.30\\
\hline
1954 & Dec & -0.18\\
\hline
1955 & Jan & 0.43\\
\hline
1955 & Feb & -0.15\\
\hline
1955 & Mar & -0.47\\
\hline
1955 & Apr & -0.24\\
\hline
1955 & May & -0.14\\
\hline
1955 & Jun & -0.09\\
\hline
1955 & Jul & -0.06\\
\hline
1955 & Aug & 0.03\\
\hline
1955 & Sep & -0.01\\
\hline
1955 & Oct & 0.11\\
\hline
1955 & Nov & -0.35\\
\hline
1955 & Dec & -0.33\\
\hline
1956 & Jan & -0.12\\
\hline
1956 & Feb & -0.36\\
\hline
1956 & Mar & -0.28\\
\hline
1956 & Apr & -0.25\\
\hline
1956 & May & -0.32\\
\hline
1956 & Jun & -0.20\\
\hline
1956 & Jul & -0.23\\
\hline
1956 & Aug & -0.27\\
\hline
1956 & Sep & -0.35\\
\hline
1956 & Oct & -0.30\\
\hline
1956 & Nov & -0.30\\
\hline
1956 & Dec & -0.16\\
\hline
1957 & Jan & -0.16\\
\hline
1957 & Feb & -0.06\\
\hline
1957 & Mar & -0.12\\
\hline
1957 & Apr & -0.10\\
\hline
1957 & May & -0.06\\
\hline
1957 & Jun & 0.11\\
\hline
1957 & Jul & 0.11\\
\hline
1957 & Aug & 0.17\\
\hline
1957 & Sep & 0.15\\
\hline
1957 & Oct & 0.08\\
\hline
1957 & Nov & 0.12\\
\hline
1957 & Dec & 0.27\\
\hline
1958 & Jan & 0.71\\
\hline
1958 & Feb & 0.33\\
\hline
1958 & Mar & 0.22\\
\hline
1958 & Apr & 0.07\\
\hline
1958 & May & 0.14\\
\hline
1958 & Jun & 0.04\\
\hline
1958 & Jul & 0.04\\
\hline
1958 & Aug & 0.15\\
\hline
1958 & Sep & 0.07\\
\hline
1958 & Oct & 0.08\\
\hline
1958 & Nov & 0.06\\
\hline
1958 & Dec & 0.18\\
\hline
1959 & Jan & 0.25\\
\hline
1959 & Feb & 0.18\\
\hline
1959 & Mar & 0.33\\
\hline
1959 & Apr & 0.21\\
\hline
1959 & May & 0.03\\
\hline
1959 & Jun & 0.15\\
\hline
1959 & Jul & 0.05\\
\hline
1959 & Aug & 0.08\\
\hline
1959 & Sep & 0.12\\
\hline
1959 & Oct & -0.05\\
\hline
1959 & Nov & 0.02\\
\hline
1959 & Dec & 0.08\\
\hline
1960 & Jan & 0.14\\
\hline
1960 & Feb & 0.48\\
\hline
1960 & Mar & -0.41\\
\hline
1960 & Apr & -0.13\\
\hline
1960 & May & 0.05\\
\hline
1960 & Jun & 0.19\\
\hline
1960 & Jul & 0.09\\
\hline
1960 & Aug & 0.11\\
\hline
1960 & Sep & 0.13\\
\hline
1960 & Oct & 0.07\\
\hline
1960 & Nov & -0.08\\
\hline
1960 & Dec & 0.40\\
\hline
1961 & Jan & 0.17\\
\hline
1961 & Feb & 0.28\\
\hline
1961 & Mar & 0.21\\
\hline
1961 & Apr & 0.14\\
\hline
1961 & May & 0.05\\
\hline
1961 & Jun & 0.19\\
\hline
1961 & Jul & 0.08\\
\hline
1961 & Aug & 0.08\\
\hline
1961 & Sep & -0.03\\
\hline
1961 & Oct & -0.03\\
\hline
1961 & Nov & 0.08\\
\hline
1961 & Dec & -0.15\\
\hline
1962 & Jan & 0.31\\
\hline
1962 & Feb & 0.38\\
\hline
1962 & Mar & 0.34\\
\hline
1962 & Apr & 0.23\\
\hline
1962 & May & 0.07\\
\hline
1962 & Jun & -0.10\\
\hline
1962 & Jul & 0.13\\
\hline
1962 & Aug & 0.07\\
\hline
1962 & Sep & 0.01\\
\hline
1962 & Oct & 0.14\\
\hline
1962 & Nov & 0.16\\
\hline
1962 & Dec & 0.15\\
\hline
1963 & Jan & 0.10\\
\hline
1963 & Feb & 0.53\\
\hline
1963 & Mar & -0.12\\
\hline
1963 & Apr & 0.06\\
\hline
1963 & May & -0.03\\
\hline
1963 & Jun & -0.01\\
\hline
1963 & Jul & 0.13\\
\hline
1963 & Aug & 0.19\\
\hline
1963 & Sep & 0.14\\
\hline
1963 & Oct & 0.42\\
\hline
1963 & Nov & 0.46\\
\hline
1963 & Dec & 0.08\\
\hline
1964 & Jan & -0.02\\
\hline
1964 & Feb & -0.07\\
\hline
1964 & Mar & -0.34\\
\hline
1964 & Apr & -0.29\\
\hline
1964 & May & -0.09\\
\hline
1964 & Jun & -0.08\\
\hline
1964 & Jul & -0.11\\
\hline
1964 & Aug & -0.22\\
\hline
1964 & Sep & -0.28\\
\hline
1964 & Oct & -0.31\\
\hline
1964 & Nov & -0.22\\
\hline
1964 & Dec & -0.28\\
\hline
1965 & Jan & -0.01\\
\hline
1965 & Feb & -0.38\\
\hline
1965 & Mar & -0.04\\
\hline
1965 & Apr & -0.26\\
\hline
1965 & May & -0.10\\
\hline
1965 & Jun & -0.20\\
\hline
1965 & Jul & -0.17\\
\hline
1965 & Aug & -0.19\\
\hline
1965 & Sep & -0.19\\
\hline
1965 & Oct & -0.03\\
\hline
1965 & Nov & 0.06\\
\hline
1965 & Dec & -0.04\\
\hline
1966 & Jan & -0.21\\
\hline
1966 & Feb & 0.10\\
\hline
1966 & Mar & 0.01\\
\hline
1966 & Apr & -0.22\\
\hline
1966 & May & -0.01\\
\hline
1966 & Jun & 0.05\\
\hline
1966 & Jul & 0.12\\
\hline
1966 & Aug & 0.16\\
\hline
1966 & Sep & 0.08\\
\hline
1966 & Oct & -0.08\\
\hline
1966 & Nov & 0.01\\
\hline
1966 & Dec & -0.09\\
\hline
1967 & Jan & -0.15\\
\hline
1967 & Feb & -0.38\\
\hline
1967 & Mar & 0.21\\
\hline
1967 & Apr & 0.06\\
\hline
1967 & May & 0.22\\
\hline
1967 & Jun & -0.05\\
\hline
1967 & Jul & 0.05\\
\hline
1967 & Aug & 0.06\\
\hline
1967 & Sep & -0.02\\
\hline
1967 & Oct & 0.30\\
\hline
1967 & Nov & 0.02\\
\hline
1967 & Dec & 0.06\\
\hline
1968 & Jan & -0.29\\
\hline
1968 & Feb & -0.18\\
\hline
1968 & Mar & 0.46\\
\hline
1968 & Apr & 0.00\\
\hline
1968 & May & -0.05\\
\hline
1968 & Jun & -0.03\\
\hline
1968 & Jul & -0.08\\
\hline
1968 & Aug & -0.09\\
\hline
1968 & Sep & -0.04\\
\hline
1968 & Oct & 0.03\\
\hline
1968 & Nov & -0.17\\
\hline
1968 & Dec & -0.30\\
\hline
1969 & Jan & -0.42\\
\hline
1969 & Feb & -0.41\\
\hline
1969 & Mar & -0.15\\
\hline
1969 & Apr & 0.06\\
\hline
1969 & May & 0.13\\
\hline
1969 & Jun & -0.05\\
\hline
1969 & Jul & 0.06\\
\hline
1969 & Aug & 0.04\\
\hline
1969 & Sep & 0.04\\
\hline
1969 & Oct & 0.03\\
\hline
1969 & Nov & 0.17\\
\hline
1969 & Dec & 0.36\\
\hline
1970 & Jan & -0.02\\
\hline
1970 & Feb & 0.31\\
\hline
1970 & Mar & -0.07\\
\hline
1970 & Apr & 0.00\\
\hline
1970 & May & -0.02\\
\hline
1970 & Jun & 0.00\\
\hline
1970 & Jul & -0.04\\
\hline
1970 & Aug & -0.06\\
\hline
1970 & Sep & -0.04\\
\hline
1970 & Oct & -0.16\\
\hline
1970 & Nov & 0.03\\
\hline
1970 & Dec & -0.30\\
\hline
1971 & Jan & -0.13\\
\hline
1971 & Feb & -0.29\\
\hline
1971 & Mar & -0.26\\
\hline
1971 & Apr & -0.20\\
\hline
1971 & May & -0.13\\
\hline
1971 & Jun & -0.14\\
\hline
1971 & Jul & -0.13\\
\hline
1971 & Aug & -0.18\\
\hline
1971 & Sep & -0.14\\
\hline
1971 & Oct & -0.16\\
\hline
1971 & Nov & 0.01\\
\hline
1971 & Dec & -0.15\\
\hline
1972 & Jan & -0.57\\
\hline
1972 & Feb & -0.52\\
\hline
1972 & Mar & -0.02\\
\hline
1972 & Apr & -0.12\\
\hline
1972 & May & -0.17\\
\hline
1972 & Jun & -0.10\\
\hline
1972 & Jul & -0.05\\
\hline
1972 & Aug & -0.07\\
\hline
1972 & Sep & -0.28\\
\hline
1972 & Oct & -0.05\\
\hline
1972 & Nov & -0.20\\
\hline
1972 & Dec & -0.04\\
\hline
1973 & Jan & 0.17\\
\hline
1973 & Feb & 0.43\\
\hline
1973 & Mar & 0.31\\
\hline
1973 & Apr & 0.17\\
\hline
1973 & May & 0.11\\
\hline
1973 & Jun & 0.16\\
\hline
1973 & Jul & 0.02\\
\hline
1973 & Aug & 0.01\\
\hline
1973 & Sep & -0.07\\
\hline
1973 & Oct & -0.02\\
\hline
1973 & Nov & -0.09\\
\hline
1973 & Dec & -0.01\\
\hline
1974 & Jan & -0.33\\
\hline
1974 & Feb & -0.38\\
\hline
1974 & Mar & 0.02\\
\hline
1974 & Apr & -0.11\\
\hline
1974 & May & -0.18\\
\hline
1974 & Jun & -0.20\\
\hline
1974 & Jul & -0.13\\
\hline
1974 & Aug & -0.11\\
\hline
1974 & Sep & -0.17\\
\hline
1974 & Oct & -0.25\\
\hline
1974 & Nov & -0.24\\
\hline
1974 & Dec & -0.29\\
\hline
1975 & Jan & 0.10\\
\hline
1975 & Feb & 0.07\\
\hline
1975 & Mar & 0.13\\
\hline
1975 & Apr & 0.06\\
\hline
1975 & May & -0.01\\
\hline
1975 & Jun & -0.06\\
\hline
1975 & Jul & -0.07\\
\hline
1975 & Aug & -0.15\\
\hline
1975 & Sep & -0.09\\
\hline
1975 & Oct & -0.15\\
\hline
1975 & Nov & -0.29\\
\hline
1975 & Dec & -0.23\\
\hline
1976 & Jan & 0.07\\
\hline
1976 & Feb & -0.12\\
\hline
1976 & Mar & -0.32\\
\hline
1976 & Apr & 0.01\\
\hline
1976 & May & -0.28\\
\hline
1976 & Jun & -0.22\\
\hline
1976 & Jul & -0.28\\
\hline
1976 & Aug & -0.23\\
\hline
1976 & Sep & -0.12\\
\hline
1976 & Oct & -0.50\\
\hline
1976 & Nov & -0.32\\
\hline
1976 & Dec & -0.26\\
\hline
1977 & Jan & -0.15\\
\hline
1977 & Feb & 0.10\\
\hline
1977 & Mar & 0.26\\
\hline
1977 & Apr & 0.28\\
\hline
1977 & May & 0.21\\
\hline
1977 & Jun & 0.20\\
\hline
1977 & Jul & 0.06\\
\hline
1977 & Aug & 0.01\\
\hline
1977 & Sep & 0.14\\
\hline
1977 & Oct & -0.04\\
\hline
1977 & Nov & 0.26\\
\hline
1977 & Dec & 0.01\\
\hline
1978 & Jan & 0.14\\
\hline
1978 & Feb & 0.13\\
\hline
1978 & Mar & 0.20\\
\hline
1978 & Apr & 0.06\\
\hline
1978 & May & -0.09\\
\hline
1978 & Jun & -0.15\\
\hline
1978 & Jul & -0.08\\
\hline
1978 & Aug & -0.14\\
\hline
1978 & Sep & -0.01\\
\hline
1978 & Oct & -0.05\\
\hline
1978 & Nov & 0.16\\
\hline
1978 & Dec & 0.07\\
\hline
1979 & Jan & 0.03\\
\hline
1979 & Feb & -0.35\\
\hline
1979 & Mar & 0.17\\
\hline
1979 & Apr & -0.15\\
\hline
1979 & May & 0.01\\
\hline
1979 & Jun & 0.06\\
\hline
1979 & Jul & 0.04\\
\hline
1979 & Aug & 0.04\\
\hline
1979 & Sep & 0.16\\
\hline
1979 & Oct & 0.21\\
\hline
1979 & Nov & 0.24\\
\hline
1979 & Dec & 0.52\\
\hline
1980 & Jan & 0.21\\
\hline
1980 & Feb & 0.43\\
\hline
1980 & Mar & 0.07\\
\hline
1980 & Apr & 0.13\\
\hline
1980 & May & 0.22\\
\hline
1980 & Jun & 0.18\\
\hline
1980 & Jul & 0.13\\
\hline
1980 & Aug & 0.10\\
\hline
1980 & Sep & 0.07\\
\hline
1980 & Oct & 0.15\\
\hline
1980 & Nov & 0.20\\
\hline
1980 & Dec & 0.06\\
\hline
1981 & Jan & 0.83\\
\hline
1981 & Feb & 0.61\\
\hline
1981 & Mar & 0.68\\
\hline
1981 & Apr & 0.38\\
\hline
1981 & May & 0.15\\
\hline
1981 & Jun & 0.30\\
\hline
1981 & Jul & 0.11\\
\hline
1981 & Aug & 0.18\\
\hline
1981 & Sep & 0.18\\
\hline
1981 & Oct & 0.24\\
\hline
1981 & Nov & 0.37\\
\hline
1981 & Dec & 0.58\\
\hline
1982 & Jan & -0.07\\
\hline
1982 & Feb & 0.16\\
\hline
1982 & Mar & 0.05\\
\hline
1982 & Apr & 0.13\\
\hline
1982 & May & 0.07\\
\hline
1982 & Jun & 0.01\\
\hline
1982 & Jul & 0.05\\
\hline
1982 & Aug & -0.07\\
\hline
1982 & Sep & 0.07\\
\hline
1982 & Oct & 0.01\\
\hline
1982 & Nov & -0.13\\
\hline
1982 & Dec & 0.37\\
\hline
1983 & Jan & 0.54\\
\hline
1983 & Feb & 0.36\\
\hline
1983 & Mar & 0.45\\
\hline
1983 & Apr & 0.09\\
\hline
1983 & May & 0.05\\
\hline
1983 & Jun & 0.12\\
\hline
1983 & Jul & 0.21\\
\hline
1983 & Aug & 0.24\\
\hline
1983 & Sep & 0.27\\
\hline
1983 & Oct & 0.11\\
\hline
1983 & Nov & 0.53\\
\hline
1983 & Dec & 0.13\\
\hline
1984 & Jan & 0.32\\
\hline
1984 & Feb & 0.06\\
\hline
1984 & Mar & 0.21\\
\hline
1984 & Apr & -0.07\\
\hline
1984 & May & 0.22\\
\hline
1984 & Jun & 0.11\\
\hline
1984 & Jul & 0.09\\
\hline
1984 & Aug & 0.03\\
\hline
1984 & Sep & -0.02\\
\hline
1984 & Oct & 0.06\\
\hline
1984 & Nov & -0.13\\
\hline
1984 & Dec & -0.42\\
\hline
1985 & Jan & 0.07\\
\hline
1985 & Feb & -0.32\\
\hline
1985 & Mar & -0.01\\
\hline
1985 & Apr & -0.07\\
\hline
1985 & May & 0.07\\
\hline
1985 & Jun & 0.03\\
\hline
1985 & Jul & -0.11\\
\hline
1985 & Aug & -0.02\\
\hline
1985 & Sep & 0.02\\
\hline
1985 & Oct & 0.00\\
\hline
1985 & Nov & 0.02\\
\hline
1985 & Dec & 0.14\\
\hline
1986 & Jan & 0.35\\
\hline
1986 & Feb & 0.34\\
\hline
1986 & Mar & 0.23\\
\hline
1986 & Apr & 0.14\\
\hline
1986 & May & 0.17\\
\hline
1986 & Jun & 0.12\\
\hline
1986 & Jul & -0.03\\
\hline
1986 & Aug & 0.03\\
\hline
1986 & Sep & -0.01\\
\hline
1986 & Oct & 0.04\\
\hline
1986 & Nov & 0.01\\
\hline
1986 & Dec & 0.08\\
\hline
1987 & Jan & 0.28\\
\hline
1987 & Feb & 0.56\\
\hline
1987 & Mar & 0.00\\
\hline
1987 & Apr & 0.07\\
\hline
1987 & May & 0.22\\
\hline
1987 & Jun & 0.18\\
\hline
1987 & Jul & 0.27\\
\hline
1987 & Aug & 0.27\\
\hline
1987 & Sep & 0.42\\
\hline
1987 & Oct & 0.23\\
\hline
1987 & Nov & 0.08\\
\hline
1987 & Dec & 0.54\\
\hline
1988 & Jan & 0.61\\
\hline
1988 & Feb & 0.44\\
\hline
1988 & Mar & 0.48\\
\hline
1988 & Apr & 0.45\\
\hline
1988 & May & 0.42\\
\hline
1988 & Jun & 0.40\\
\hline
1988 & Jul & 0.41\\
\hline
1988 & Aug & 0.27\\
\hline
1988 & Sep & 0.27\\
\hline
1988 & Oct & 0.24\\
\hline
1988 & Nov & 0.04\\
\hline
1988 & Dec & 0.46\\
\hline
1989 & Jan & 0.10\\
\hline
1989 & Feb & 0.47\\
\hline
1989 & Mar & 0.43\\
\hline
1989 & Apr & 0.34\\
\hline
1989 & May & 0.21\\
\hline
1989 & Jun & 0.26\\
\hline
1989 & Jul & 0.32\\
\hline
1989 & Aug & 0.28\\
\hline
1989 & Sep & 0.28\\
\hline
1989 & Oct & 0.27\\
\hline
1989 & Nov & 0.12\\
\hline
1989 & Dec & 0.36\\
\hline
1990 & Jan & 0.45\\
\hline
1990 & Feb & 0.50\\
\hline
1990 & Mar & 1.15\\
\hline
1990 & Apr & 0.66\\
\hline
1990 & May & 0.47\\
\hline
1990 & Jun & 0.49\\
\hline
1990 & Jul & 0.31\\
\hline
1990 & Aug & 0.36\\
\hline
1990 & Sep & 0.41\\
\hline
1990 & Oct & 0.46\\
\hline
1990 & Nov & 0.48\\
\hline
1990 & Dec & 0.37\\
\hline
1991 & Jan & 0.55\\
\hline
1991 & Feb & 0.52\\
\hline
1991 & Mar & 0.46\\
\hline
1991 & Apr & 0.55\\
\hline
1991 & May & 0.38\\
\hline
1991 & Jun & 0.45\\
\hline
1991 & Jul & 0.45\\
\hline
1991 & Aug & 0.35\\
\hline
1991 & Sep & 0.38\\
\hline
1991 & Oct & 0.29\\
\hline
1991 & Nov & 0.35\\
\hline
1991 & Dec & 0.21\\
\hline
1992 & Jan & 0.54\\
\hline
1992 & Feb & 0.49\\
\hline
1992 & Mar & 0.50\\
\hline
1992 & Apr & 0.10\\
\hline
1992 & May & 0.18\\
\hline
1992 & Jun & 0.04\\
\hline
1992 & Jul & -0.09\\
\hline
1992 & Aug & -0.08\\
\hline
1992 & Sep & -0.20\\
\hline
1992 & Oct & -0.14\\
\hline
1992 & Nov & -0.09\\
\hline
1992 & Dec & 0.23\\
\hline
1993 & Jan & 0.42\\
\hline
1993 & Feb & 0.53\\
\hline
1993 & Mar & 0.39\\
\hline
1993 & Apr & 0.20\\
\hline
1993 & May & 0.29\\
\hline
1993 & Jun & 0.22\\
\hline
1993 & Jul & 0.11\\
\hline
1993 & Aug & 0.01\\
\hline
1993 & Sep & -0.07\\
\hline
1993 & Oct & 0.13\\
\hline
1993 & Nov & -0.16\\
\hline
1993 & Dec & 0.19\\
\hline
1994 & Jan & 0.36\\
\hline
1994 & Feb & -0.02\\
\hline
1994 & Mar & 0.43\\
\hline
1994 & Apr & 0.47\\
\hline
1994 & May & 0.37\\
\hline
1994 & Jun & 0.41\\
\hline
1994 & Jul & 0.34\\
\hline
1994 & Aug & 0.31\\
\hline
1994 & Sep & 0.29\\
\hline
1994 & Oct & 0.53\\
\hline
1994 & Nov & 0.52\\
\hline
1994 & Dec & 0.34\\
\hline
1995 & Jan & 0.74\\
\hline
1995 & Feb & 1.16\\
\hline
1995 & Mar & 0.57\\
\hline
1995 & Apr & 0.74\\
\hline
1995 & May & 0.40\\
\hline
1995 & Jun & 0.47\\
\hline
1995 & Jul & 0.40\\
\hline
1995 & Aug & 0.56\\
\hline
1995 & Sep & 0.43\\
\hline
1995 & Oct & 0.61\\
\hline
1995 & Nov & 0.57\\
\hline
1995 & Dec & 0.31\\
\hline
1996 & Jan & 0.29\\
\hline
1996 & Feb & 0.58\\
\hline
1996 & Mar & 0.30\\
\hline
1996 & Apr & 0.22\\
\hline
1996 & May & 0.38\\
\hline
1996 & Jun & 0.26\\
\hline
1996 & Jul & 0.28\\
\hline
1996 & Aug & 0.17\\
\hline
1996 & Sep & 0.06\\
\hline
1996 & Oct & 0.04\\
\hline
1996 & Nov & 0.37\\
\hline
1996 & Dec & 0.40\\
\hline
1997 & Jan & 0.37\\
\hline
1997 & Feb & 0.56\\
\hline
1997 & Mar & 0.78\\
\hline
1997 & Apr & 0.53\\
\hline
1997 & May & 0.43\\
\hline
1997 & Jun & 0.50\\
\hline
1997 & Jul & 0.43\\
\hline
1997 & Aug & 0.46\\
\hline
1997 & Sep & 0.54\\
\hline
1997 & Oct & 0.67\\
\hline
1997 & Nov & 0.60\\
\hline
1997 & Dec & 0.52\\
\hline
1998 & Jan & 0.67\\
\hline
1998 & Feb & 1.07\\
\hline
1998 & Mar & 0.73\\
\hline
1998 & Apr & 0.88\\
\hline
1998 & May & 0.65\\
\hline
1998 & Jun & 0.73\\
\hline
1998 & Jul & 0.77\\
\hline
1998 & Aug & 0.74\\
\hline
1998 & Sep & 0.60\\
\hline
1998 & Oct & 0.56\\
\hline
1998 & Nov & 0.58\\
\hline
1998 & Dec & 0.74\\
\hline
1999 & Jan & 0.59\\
\hline
1999 & Feb & 1.01\\
\hline
1999 & Mar & 0.31\\
\hline
1999 & Apr & 0.54\\
\hline
1999 & May & 0.41\\
\hline
1999 & Jun & 0.36\\
\hline
1999 & Jul & 0.35\\
\hline
1999 & Aug & 0.34\\
\hline
1999 & Sep & 0.40\\
\hline
1999 & Oct & 0.42\\
\hline
1999 & Nov & 0.55\\
\hline
1999 & Dec & 0.70\\
\hline
2000 & Jan & 0.34\\
\hline
2000 & Feb & 0.87\\
\hline
2000 & Mar & 0.87\\
\hline
2000 & Apr & 0.87\\
\hline
2000 & May & 0.56\\
\hline
2000 & Jun & 0.46\\
\hline
2000 & Jul & 0.41\\
\hline
2000 & Aug & 0.48\\
\hline
2000 & Sep & 0.38\\
\hline
2000 & Oct & 0.30\\
\hline
2000 & Nov & 0.19\\
\hline
2000 & Dec & 0.29\\
\hline
2001 & Jan & 0.54\\
\hline
2001 & Feb & 0.46\\
\hline
2001 & Mar & 0.76\\
\hline
2001 & Apr & 0.57\\
\hline
2001 & May & 0.62\\
\hline
2001 & Jun & 0.55\\
\hline
2001 & Jul & 0.64\\
\hline
2001 & Aug & 0.68\\
\hline
2001 & Sep & 0.55\\
\hline
2001 & Oct & 0.61\\
\hline
2001 & Nov & 0.95\\
\hline
2001 & Dec & 0.66\\
\hline
2002 & Jan & 0.99\\
\hline
2002 & Feb & 1.07\\
\hline
2002 & Mar & 1.13\\
\hline
2002 & Apr & 0.49\\
\hline
2002 & May & 0.50\\
\hline
2002 & Jun & 0.72\\
\hline
2002 & Jul & 0.68\\
\hline
2002 & Aug & 0.53\\
\hline
2002 & Sep & 0.60\\
\hline
2002 & Oct & 0.56\\
\hline
2002 & Nov & 0.77\\
\hline
2002 & Dec & 0.48\\
\hline
2003 & Jan & 0.90\\
\hline
2003 & Feb & 0.51\\
\hline
2003 & Mar & 0.61\\
\hline
2003 & Apr & 0.59\\
\hline
2003 & May & 0.70\\
\hline
2003 & Jun & 0.54\\
\hline
2003 & Jul & 0.66\\
\hline
2003 & Aug & 0.84\\
\hline
2003 & Sep & 0.79\\
\hline
2003 & Oct & 0.97\\
\hline
2003 & Nov & 0.66\\
\hline
2003 & Dec & 0.96\\
\hline
2004 & Jan & 0.68\\
\hline
2004 & Feb & 0.89\\
\hline
2004 & Mar & 0.88\\
\hline
2004 & Apr & 0.74\\
\hline
2004 & May & 0.55\\
\hline
2004 & Jun & 0.47\\
\hline
2004 & Jul & 0.49\\
\hline
2004 & Aug & 0.54\\
\hline
2004 & Sep & 0.61\\
\hline
2004 & Oct & 0.76\\
\hline
2004 & Nov & 0.94\\
\hline
2004 & Dec & 0.47\\
\hline
2005 & Jan & 0.90\\
\hline
2005 & Feb & 0.68\\
\hline
2005 & Mar & 0.83\\
\hline
2005 & Apr & 0.90\\
\hline
2005 & May & 0.76\\
\hline
2005 & Jun & 0.79\\
\hline
2005 & Jul & 0.76\\
\hline
2005 & Aug & 0.70\\
\hline
2005 & Sep & 0.91\\
\hline
2005 & Oct & 0.94\\
\hline
2005 & Nov & 1.00\\
\hline
2005 & Dec & 0.78\\
\hline
2006 & Jan & 0.65\\
\hline
2006 & Feb & 0.88\\
\hline
2006 & Mar & 0.78\\
\hline
2006 & Apr & 0.60\\
\hline
2006 & May & 0.72\\
\hline
2006 & Jun & 0.77\\
\hline
2006 & Jul & 0.66\\
\hline
2006 & Aug & 0.65\\
\hline
2006 & Sep & 0.73\\
\hline
2006 & Oct & 0.95\\
\hline
2006 & Nov & 0.91\\
\hline
2006 & Dec & 1.08\\
\hline
2007 & Jan & 1.32\\
\hline
2007 & Feb & 0.92\\
\hline
2007 & Mar & 0.91\\
\hline
2007 & Apr & 1.04\\
\hline
2007 & May & 0.68\\
\hline
2007 & Jun & 0.63\\
\hline
2007 & Jul & 0.62\\
\hline
2007 & Aug & 0.69\\
\hline
2007 & Sep & 0.65\\
\hline
2007 & Oct & 0.82\\
\hline
2007 & Nov & 0.77\\
\hline
2007 & Dec & 0.71\\
\hline
2008 & Jan & 0.21\\
\hline
2008 & Feb & 0.51\\
\hline
2008 & Mar & 1.08\\
\hline
2008 & Apr & 0.62\\
\hline
2008 & May & 0.57\\
\hline
2008 & Jun & 0.60\\
\hline
2008 & Jul & 0.57\\
\hline
2008 & Aug & 0.58\\
\hline
2008 & Sep & 0.57\\
\hline
2008 & Oct & 0.83\\
\hline
2008 & Nov & 0.91\\
\hline
2008 & Dec & 0.66\\
\hline
2009 & Jan & 0.79\\
\hline
2009 & Feb & 0.68\\
\hline
2009 & Mar & 0.57\\
\hline
2009 & Apr & 0.65\\
\hline
2009 & May & 0.64\\
\hline
2009 & Jun & 0.67\\
\hline
2009 & Jul & 0.62\\
\hline
2009 & Aug & 0.69\\
\hline
2009 & Sep & 0.78\\
\hline
2009 & Oct & 0.75\\
\hline
2009 & Nov & 0.79\\
\hline
2009 & Dec & 0.68\\
\hline
2010 & Jan & 0.78\\
\hline
2010 & Feb & 0.89\\
\hline
2010 & Mar & 1.11\\
\hline
2010 & Apr & 1.13\\
\hline
2010 & May & 0.90\\
\hline
2010 & Jun & 0.78\\
\hline
2010 & Jul & 0.83\\
\hline
2010 & Aug & 0.80\\
\hline
2010 & Sep & 0.63\\
\hline
2010 & Oct & 0.87\\
\hline
2010 & Nov & 1.15\\
\hline
2010 & Dec & 0.54\\
\hline
2011 & Jan & 0.54\\
\hline
2011 & Feb & 0.60\\
\hline
2011 & Mar & 0.85\\
\hline
2011 & Apr & 0.79\\
\hline
2011 & May & 0.61\\
\hline
2011 & Jun & 0.72\\
\hline
2011 & Jul & 0.69\\
\hline
2011 & Aug & 0.66\\
\hline
2011 & Sep & 0.68\\
\hline
2011 & Oct & 0.87\\
\hline
2011 & Nov & 0.62\\
\hline
2011 & Dec & 0.77\\
\hline
2012 & Jan & 0.62\\
\hline
2012 & Feb & 0.56\\
\hline
2012 & Mar & 0.69\\
\hline
2012 & Apr & 1.01\\
\hline
2012 & May & 0.91\\
\hline
2012 & Jun & 0.87\\
\hline
2012 & Jul & 0.79\\
\hline
2012 & Aug & 0.69\\
\hline
2012 & Sep & 0.83\\
\hline
2012 & Oct & 0.84\\
\hline
2012 & Nov & 0.90\\
\hline
2012 & Dec & 0.47\\
\hline
2013 & Jan & 0.79\\
\hline
2013 & Feb & 0.65\\
\hline
2013 & Mar & 0.79\\
\hline
2013 & Apr & 0.65\\
\hline
2013 & May & 0.75\\
\hline
2013 & Jun & 0.72\\
\hline
2013 & Jul & 0.67\\
\hline
2013 & Aug & 0.67\\
\hline
2013 & Sep & 0.69\\
\hline
2013 & Oct & 0.81\\
\hline
2013 & Nov & 1.05\\
\hline
2013 & Dec & 0.79\\
\hline
2014 & Jan & 0.94\\
\hline
2014 & Feb & 0.67\\
\hline
2014 & Mar & 1.14\\
\hline
2014 & Apr & 1.03\\
\hline
2014 & May & 0.88\\
\hline
2014 & Jun & 0.82\\
\hline
2014 & Jul & 0.73\\
\hline
2014 & Aug & 0.89\\
\hline
2014 & Sep & 0.84\\
\hline
2014 & Oct & 0.96\\
\hline
2014 & Nov & 0.81\\
\hline
2014 & Dec & 1.09\\
\hline
2015 & Jan & 1.13\\
\hline
2015 & Feb & 1.13\\
\hline
2015 & Mar & 1.22\\
\hline
2015 & Apr & 1.02\\
\hline
2015 & May & 0.96\\
\hline
2015 & Jun & 1.03\\
\hline
2015 & Jul & 0.90\\
\hline
2015 & Aug & 1.00\\
\hline
2015 & Sep & 1.13\\
\hline
2015 & Oct & 1.24\\
\hline
2015 & Nov & 1.35\\
\hline
2015 & Dec & 1.45\\
\hline
2016 & Jan & 1.55\\
\hline
2016 & Feb & 1.92\\
\hline
2016 & Mar & 1.83\\
\hline
2016 & Apr & 1.42\\
\hline
2016 & May & 1.03\\
\hline
2016 & Jun & 1.10\\
\hline
2016 & Jul & 1.02\\
\hline
2016 & Aug & 1.08\\
\hline
2016 & Sep & 1.18\\
\hline
2016 & Oct & 1.05\\
\hline
2016 & Nov & 1.11\\
\hline
2016 & Dec & 1.02\\
\hline
2017 & Jan & 1.29\\
\hline
2017 & Feb & 1.47\\
\hline
2017 & Mar & 1.44\\
\hline
2017 & Apr & 1.15\\
\hline
2017 & May & 0.85\\
\hline
2017 & Jun & 0.90\\
\hline
2017 & Jul & 0.95\\
\hline
2017 & Aug & 0.97\\
\hline
2017 & Sep & 0.97\\
\hline
2017 & Oct & 1.02\\
\hline
2017 & Nov & 1.19\\
\hline
2017 & Dec & 1.31\\
\hline
2018 & Jan & 1.06\\
\hline
2018 & Feb & 1.24\\
\hline
2018 & Mar & 1.19\\
\hline
2018 & Apr & 0.91\\
\hline
2018 & May & 0.94\\
\hline
2018 & Jun & 0.82\\
\hline
2018 & Jul & 0.81\\
\hline
2018 & Aug & 0.81\\
\hline
2018 & Sep & 0.91\\
\hline
2018 & Oct & 1.20\\
\hline
2018 & Nov & 0.91\\
\hline
2018 & Dec & 1.07\\
\hline
2019 & Jan & 1.11\\
\hline
2019 & Feb & 1.06\\
\hline
2019 & Mar & 1.44\\
\hline
2019 & Apr & 1.14\\
\hline
2019 & May & 0.94\\
\hline
2019 & Jun & 1.15\\
\hline
2019 & Jul & 0.98\\
\hline
2019 & Aug & NA\\
\hline
2019 & Sep & NA\\
\hline
2019 & Oct & NA\\
\hline
2019 & Nov & NA\\
\hline
2019 & Dec & NA\\
\hline
\end{tabular}
\end{table}

Let us plot the data using a time-series scatter plot, and add a trendline. To do that, we first need to create a new variable called `date` in order to ensure that the `delta` values are plot chronologically.


```r
tidyweather <- tidyweather %>%
  mutate(date = ymd(paste(as.character(Year), month, "1")),
         month = month(date),
         year = year(date))

ggplot(tidyweather, aes(x=date, y = delta, colour = date)) +
  geom_point() +
  geom_smooth(color="white") + 
  theme(plot.title = element_text(face = "bold", size = 18)) + 
  theme_bw() + 
  scale_colour_gradient(low="blue2", high="red") +
  labs (
    title = "Temperature Anomalies Heating Up Over Past Century", 
  subtitle = "Combined land-surface air and sea-surface water temperature anomalies in the northern \nhemisphere have shifted from negative to positive in the last 100 years", x = "Year", y = "Delta", caption = "Source: NASA's Goddard Institute for Space Studies") +
  theme(legend.position = "none", plot.title = element_text(face = "bold"), axis.text=element_text(size=12),
        axis.title=element_text(size=14,face="bold"))
```



\begin{center}\includegraphics{blog1_files/figure-latex/scatter_plot-1} \end{center}

On analysing the temperature difference through different months, we find that the effects of increasing temperature appear to be homogeneous throughout different months. 


```r
## add to the title/sub , can i thin the smoothing line? , make the theme prettier (no grey months)
ggplot(tidyweather, aes(x=date, y = delta, colour = date))+
  geom_point()+
  geom_smooth(color="white") +
  theme_bw() +
  scale_colour_gradient(low="blue2", high="red") +
  labs (
    title = "Monthly Anomalies Appear Homogeneous Over Time ", 
  subtitle = "Anomalies appear not to discriminate based on month, with monthly patterns being nearly \nindistinguishable from one another", x = "Year", y = "Delta", caption = "Source: NASA's Goddard Institute for Space Studies") +
  facet_wrap(~month) + 
  theme(legend.position = "none", plot.title = element_text(face = "bold"), axis.text=element_text(size=12),
        axis.title=element_text(size=14,face="bold"))
```



\begin{center}\includegraphics{blog1_files/figure-latex/facet_wrap-1} \end{center}

Here, the years are divided into intervals to help study the data better:



```r
comparison <- tidyweather %>% 
  filter(year>= 1881) %>%     #remove years prior to 1881
  #create new variable 'interval', and assign values based on criteria below:
  mutate(interval = case_when(
    Year %in% c(1881:1920) ~ "1881-1920",
    Year %in% c(1921:1950) ~ "1921-1950",
    Year %in% c(1951:1980) ~ "1951-1980",
    Year %in% c(1981:2010) ~ "1981-2010",
    TRUE ~ "2011-present"
  ))
```


Now that we have the `interval` variable, we can create a density plot to study the distribution of monthly deviations (`delta`), grouped by the different time periods we are interested in. Set `fill` to `interval` to group and colour the data by different time periods.


```r
## fix the colors 
ggplot(comparison, aes(x=delta, fill=interval, colour = interval))+
  
  #density plot with tranparency set to 20%
  
  geom_density(alpha=0.2) +   
  theme_bw() +                
  labs(
    title = "Density Plot for Monthly Temperature Anomalies",
    subtitle = "A clear increase in temperature anomalies shown through time periods \nfrom 1881 to present day",
    x = "Delta",
    y     = "Density",
    caption = "Source: NASA's Goddard Institute for Space Studies")  + 
  scale_fill_manual(values = c("blue2", "darkorchid4", "purple1", "red1", "red"))  + 
  scale_color_manual(values = c("blue2", "darkorchid4", "purple1", "red1", "red")) +
  theme(plot.title = element_text(face = "bold"), axis.text=element_text(size=12),
        axis.title=element_text(size=14,face="bold"))+
  guides(color=guide_legend("Time Period"),fill=guide_legend("Time Period"))
```

So far, we have been working with monthly anomalies. However, we might be interested in average annual anomalies. We can do this by using `group_by()` and `summarise()`, followed by a scatter plot to display the result. 


```r
#creating yearly averages
average_annual_anomaly <- tidyweather %>% 
  group_by(year) %>%   #grouping data by Year
  
  # creating summaries for mean delta 
  # use `na.rm=TRUE` to eliminate NA (not available) values 
  summarise(annual_average_anomaly = mean(delta, na.rm=TRUE)) 

#plotting the data:
ggplot(average_annual_anomaly, aes(x=year, y= annual_average_anomaly, colour = year))+
  geom_point()+
  
  #Fit the best fit line, using LOESS method
  geom_smooth(colour = "white") +
  
  #change to theme_bw() to have white background + black frame around plot
  theme_bw() +
  
    ## add color gradient
  scale_colour_gradient(low="blue2", high="red") +
  
  labs (
    title = "Average Yearly Temperature Anomaly Signifies Steep Temperature Increase in Past 50 Years",
    subtitle = "Yearly temperature anomaly has accelerated significantly in more recent years, raising concern among climate scientists",
    y = "Average Annual Delta",
    x = "Year",
    caption = "Source: NASA's Goddard Institute for Space Studies")  +
  
  theme(legend.position = "none", plot.title = element_text(face = "bold"), axis.text=element_text(size=12),
        axis.title=element_text(size=14,face="bold"))
## make more clear 
```
