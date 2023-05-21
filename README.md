# community-movie-dashboard
import pandas as pd
import plotly.express as px
movies = pd.read_csv('Bollywood_movies.csv')
movies.info()
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 25 entries, 0 to 24
Data columns (total 9 columns):
 #   Column                        Non-Null Count  Dtype  
---  ------                        --------------  -----  
 0   Rank                          25 non-null     int64  
 1   Peak                          25 non-null     int64  
 2   Film                          25 non-null     object 
 3   Year                          25 non-null     int64  
 4   Director                      25 non-null     object 
 5   Producer                      25 non-null     object 
 6   Language                      25 non-null     object 
 7   Worldwide_Gross_INR(Crore)    25 non-null     float64
 8   Worldwide_Gross_USD(Million)  25 non-null     float64
dtypes: float64(2), int64(3), object(4)
memory usage: 1.9+ KB
movies.head(10)
Rank	Peak	Film	Year	Director	Producer	Language	Worldwide_Gross_INR(Crore)	Worldwide_Gross_USD(Million)
0	1	1	Dangal	2016	Nitesh Tiwari	Aamir Khan Productions UTV Motion Pictures W...	Hindi	2024.00	311.00
1	2	1	Baahubali 2: The Conclusion	2017	S. S. Rajamouli	Arka Media Works	Telugu	1810.00	278.00
2	3	3	Bajrangi Bhaijaan	2015	Kabir Khan	Salman Khan Films Kabir Khan Films Eros Inte...	Hindi	969.06	150.00
3	4	3	Secret Superstar	2017	Advait Chandan	Aamir Khan Productions	Hindi	966.86	154.00
4	5	1	PK	2014	Rajkumar Hirani	Vinod Chopra Films Rajkumar Hirani Films	Hindi	832.00	120.00
5	6	5	2.0	2018	S.Shankar	Lyca Productions	Tamil	800.00	123.00
6	7	2	Baahubali: The Beginning	2015	S. S. Rajamouli	Arka Media Works	Telugu	650.00	101.00
7	8	4	Sultan	2016	Ali Abbas Zafar	Yash Raj Films	Hindi	623.33	96.00
8	9	8	Sanju	2018	Rajkumar Hirani	Rajkumar Hirani FilmsVinod Chopra Films	Hindi	586.85	90.12
9	10	7	Padmaavat	2018	Sanjay Leela Bhansali	Bhansali Productions Viacom 18 Motion Pictures	Hindi	585.00	90.00
fig = px.bar(movies, x="Film", y="Worldwide_Gross_INR(Crore)", color='Rank', title='Top Grossing Bolywood Movies')
fig.show()
Movies arranges by year
movies_year = movies.groupby("Year", as_index=False).sum()
movies_year = movies_year.drop(columns=['Rank','Peak'])
movies_year = movies_year.sort_values(by=["Year"],ascending= False)
movies_year
Year	Worldwide_Gross_INR(Crore)	Worldwide_Gross_USD(Million)
8	2020	366.36	56.00
7	2019	1276.24	184.00
6	2018	2828.74	428.12
5	2017	3341.96	519.32
4	2016	2647.33	407.00
3	2015	2427.91	376.00
2	2014	1631.21	251.08
1	2013	1372.37	234.19
0	2009	460.00	88.00
movies_year1 = movies['Year'].value_counts()
movies_year1
2018    5
2015    4
2014    3
2013    3
2019    3
2017    3
2016    2
2009    1
2020    1
Name: Year, dtype: int64
movies_year.insert(1, "Number_of_Movies", [1,3,5,3,2,4,3,3,1], True) 
movies_year
Year	Number_of_Movies	Worldwide_Gross_INR(Crore)	Worldwide_Gross_USD(Million)
8	2020	1	366.36	56.00
7	2019	3	1276.24	184.00
6	2018	5	2828.74	428.12
5	2017	3	3341.96	519.32
4	2016	2	2647.33	407.00
3	2015	4	2427.91	376.00
2	2014	3	1631.21	251.08
1	2013	3	1372.37	234.19
0	2009	1	460.00	88.00
fig = px.scatter(movies_year, x="Year", y="Number_of_Movies", size='Number_of_Movies', color='Year', title='Number of Top Grossing Movies Produced Each Year')
fig.show()
Directors
movies_director = movies['Director'].value_counts()
movies_director
Rohit Shetty             3
Rajkumar Hirani          3
S. S. Rajamouli          2
Ali Abbas Zafar          2
Sandeep Vanga            1
Vijay Krishna Acharya    1
Rakesh Roshan            1
Kabir Khan               1
Nitesh Tiwari            1
Farah Khan               1
Sajid Nadiadwala         1
S.Shankar                1
Sriram Raghavan          1
Sujeeth                  1
Sooraj R. Barjatya       1
Advait Chandan           1
Om Raut                  1
Sanjay Leela Bhansali    1
Siddharth Anand          1
Name: Director, dtype: int64
movies_directors = movies.groupby("Director", as_index=False).sum()
movies_directors.insert(1, "Number_of_Movies_Directed", [1,2,1,1,1,1,3,1,3,2,1,1,1,1,1,1,1,1,1], True) 
movies_directors = movies_directors.drop(columns=['Rank','Peak','Year'])
movies_directors.head()
Director	Number_of_Movies_Directed	Worldwide_Gross_INR(Crore)	Worldwide_Gross_USD(Million)
0	Advait Chandan	1	966.86	154.00
1	Ali Abbas Zafar	2	1188.43	183.32
2	Farah Khan	1	397.21	65.08
3	Kabir Khan	1	969.06	150.00
4	Nitesh Tiwari	1	2024.00	311.00
fig = px.scatter(movies_directors, x="Director", y="Worldwide_Gross_INR(Crore)", size='Number_of_Movies_Directed', color='Number_of_Movies_Directed', title='Number of Top Grossing Movies Directed')

fig.show()
Language
movies_langauge = movies['Language'].value_counts()
movies_langauge
Hindi     21
Telugu     3
Tamil      1
Name: Language, dtype: int64
movies_langauge = movies.groupby("Language", as_index=False).sum()
movies_langauge.insert(1, "Number_of_Movies", [21,1,3], True) 
movies_langauge = movies_langauge.drop(columns=['Rank','Peak','Year','Worldwide_Gross_USD(Million)'])

movies_langauge.head()
Language	Number_of_Movies	Worldwide_Gross_INR(Crore)
0	Hindi	21	12659.06
1	Tamil	1	800.00
2	Telugu	3	2893.06
fig = px.pie(movies_langauge, values='Number_of_Movies', names='Language', title ="Language of Top Grossing Movies", color="Language", 
            color_discrete_map={'Hindi':'darkblue',
                                 'Tamil':'mediumvioletred',
                                 'Telugu':'gold'})
fig.show()


Rank	Peak	Film	Year	Director	Producer	Language	Worldwide_Gross_INR(Crore)	Worldwide_Gross_USD(Million)
1	1	Dangal	2016	Nitesh Tiwari	Aamir Khan Productions UTV Motion Pictures Walt Disney Studios India	Hindi	2024.00	311.00
2	1	Baahubali 2: The Conclusion	2017	S. S. Rajamouli	Arka Media Works	Telugu	1810.00	278.00
3	3	Bajrangi Bhaijaan	2015	Kabir Khan	Salman Khan Films Kabir Khan Films Eros International	Hindi	969.06	150.00
4	3	Secret Superstar	2017	Advait Chandan	Aamir Khan Productions	Hindi	966.86	154.00
5	1	PK	2014	Rajkumar Hirani	Vinod Chopra Films Rajkumar Hirani Films	Hindi	832.00	120.00
6	5	2.0	2018	S.Shankar	Lyca Productions	Tamil	800.00	123.00
7	2	Baahubali: The Beginning	2015	S. S. Rajamouli	Arka Media Works	Telugu	650.00	101.00
8	4	Sultan	2016	Ali Abbas Zafar	Yash Raj Films	Hindi	623.33	96.00
9	8	Sanju	2018	Rajkumar Hirani	Rajkumar Hirani FilmsVinod Chopra Films	Hindi	586.85	90.12
10	7	Padmaavat	2018	Sanjay Leela Bhansali	Bhansali Productions Viacom 18 Motion Pictures	Hindi	585.00	90.00
11	8	Tiger Zinda Hai	2017	Ali Abbas Zafar	Yash Raj Films	Hindi	565.10	87.32
12	1	Dhoom 3	2013	Vijay Krishna Acharya	Yash Raj Films	Hindi	556.00	101.00
13	9	War	2019	Siddharth Anand	Yash Raj Films	Hindi	475.50	67.00
14	1	3 Idiots	2009	Rajkumar Hirani	Vinod Chopra Films	Hindi	460.00	88.00
15	14	Andhadhun	2018	Sriram Raghavan	Viacom 18 Motion Pictures Matchbox Pictures	Hindi	456.89	64.00
16	16	Saaho	2019	Sujeeth	UV Creations T-Series	Telugu	433.06	61.00
17	6	Prem Ratan Dhan Payo	2015	Sooraj R. Barjatya	Fox Star Studios Rajshri Productions	Hindi	432.00	67.00
18	2	Chennai Express	2013	Rohit Shetty	Red Chillies Entertainment	Hindi	423.00	72.19
19	4	Kick	2014	Sajid Nadiadwala	Nadiadwala Grandson	Hindi	402.00	66.00
20	17	Simmba	2018	Rohit Shetty	Reliance Entertainment Dharma Productions	Hindi	400.00	61.00
21	5	Happy New Year	2014	Farah Khan	Red Chillies Entertainment	Hindi	397.21	65.08
22	8	Krrish 3	2013	Rakesh Roshan	Filmkraft Productions Pvt. Ltd	Hindi	393.37	61.00
23	10	Dilwale	2015	Rohit Shetty	Red Chillies Entertainment Rohit Shetty Productions	Hindi	376.85	58.00
24	21	Kabir Singh	2019	Sandeep Vanga	Cine1 Studios T-Series	Hindi	367.68	56.00
25	25	Tanhaji	2020	Om Raut	Ajay Devgn FFilmsT-Series	Hindi	366.36	56.00
import pandas as pd
import numpy as np
import requests
from bs4 import BeautifulSoup
Web Scraping
website='https://en.wikipedia.org/wiki/List_of_highest-grossing_Indian_films'
website_url=requests.get(website).text
soup=BeautifulSoup(website_url,'html.parser')
s = requests.Session() 
response = s.get(website, timeout=10) 
response
<Response [200]>
my_table = soup.find('table', {"class":'wikitable sortable'})
my_table
<table class="wikitable sortable" style="margin:auto; margin:auto;">
<tbody><tr>
<th>Rank
</th>
<th>Peak
</th>
<th>Film
</th>
<th>Year
</th>
<th>Director
</th>
<th>Studio(s)
</th>
<th>Primary <br/> language
</th>
<th>Worldwide gross
</th>
<th>Source
</th></tr>
<tr>
<td>1
</td>
<td>1
</td>
<td><i><a href="/wiki/Dangal_(film)" title="Dangal (film)">Dangal</a></i>
</td>
<td>2016
</td>
<td><a href="/wiki/Nitesh_Tiwari" title="Nitesh Tiwari">Nitesh Tiwari</a>
</td>
<td><a href="/wiki/Aamir_Khan_Productions" title="Aamir Khan Productions">Aamir Khan Productions</a> <br/> <a href="/wiki/UTV_Motion_Pictures" title="UTV Motion Pictures">UTV Motion Pictures</a> <br/> <a href="/wiki/The_Walt_Disney_Company_India" title="The Walt Disney Company India">Walt Disney Studios India</a>
</td>
<td><a href="/wiki/Hindi" title="Hindi">Hindi</a>
</td>
<td><span data-sort-value="311 !"></span> <span style="white-space: nowrap">₹2,024 <a href="/wiki/Crore" title="Crore">crore</a></span> (<span style="white-space: nowrap">US$311 million</span>)
</td>
<td><sup class="reference" id="cite_ref-boi-worldwide_9-0"><a href="#cite_note-boi-worldwide-9">[9]</a></sup>
</td></tr>
<tr>
<td>2
</td>
<td>1
</td>
<td><i><a href="/wiki/Baahubali_2:_The_Conclusion" title="Baahubali 2: The Conclusion">Baahubali 2: The Conclusion</a></i>
</td>
<td>2017
</td>
<td><a href="/wiki/S._S._Rajamouli" title="S. S. Rajamouli">S. S. Rajamouli</a>
</td>
<td><a href="/wiki/Arka_Media_Works" title="Arka Media Works">Arka Media Works</a>
</td>
<td><a href="/wiki/Telugu_language" title="Telugu language">Telugu</a>
</td>
<td><span data-sort-value="278 !"></span> <span style="white-space: nowrap">₹1,810 crore</span> (<span style="white-space: nowrap">US$278 million</span>)
</td>
<td><sup class="reference" id="cite_ref-boi-worldwide_9-1"><a href="#cite_note-boi-worldwide-9">[9]</a></sup>
</td></tr>
<tr>
<td>3
</td>
<td>3
</td>
<td><i><a href="/wiki/Bajrangi_Bhaijaan" title="Bajrangi Bhaijaan">Bajrangi Bhaijaan</a></i>
</td>
<td>2015
</td>
<td><a href="/wiki/Kabir_Khan_(director)" title="Kabir Khan (director)">Kabir Khan</a>
</td>
<td><a href="/wiki/Salman_Khan" title="Salman Khan">Salman Khan Films</a> <br/> <a href="/wiki/Kabir_Khan_(director)" title="Kabir Khan (director)">Kabir Khan Films</a> <br/> <a href="/wiki/Eros_International" title="Eros International">Eros International</a>
</td>
<td>Hindi
</td>
<td><span data-sort-value="150 !"></span> <span style="white-space: nowrap">₹969.06 crore</span> (<span style="white-space: nowrap">US$150 million</span>)
</td>
<td><sup class="reference" id="cite_ref-Bajrangi_18-0"><a href="#cite_note-Bajrangi-18">[n 1]</a></sup>
</td></tr>
<tr>
<td>4
</td>
<td>3
</td>
<td><i><a href="/wiki/Secret_Superstar" title="Secret Superstar">Secret Superstar</a></i>
</td>
<td>2017
</td>
<td><a href="/wiki/Advait_Chandan" title="Advait Chandan">Advait Chandan</a>
</td>
<td>Aamir Khan Productions
</td>
<td>Hindi
</td>
<td><span data-sort-value="154 !"></span> <span style="white-space: nowrap">₹966.86 crore</span> (<span style="white-space: nowrap">US$154 million</span>)
</td>
<td><sup class="reference" id="cite_ref-SecretSuperstar_21-0"><a href="#cite_note-SecretSuperstar-21">[n 2]</a></sup>
</td></tr>
<tr>
<td>5
</td>
<td>1
</td>
<td><i><a href="/wiki/PK_(film)" title="PK (film)">PK</a></i>
</td>
<td>2014
</td>
<td><a href="/wiki/Rajkumar_Hirani" title="Rajkumar Hirani">Rajkumar Hirani</a>
</td>
<td><a href="/wiki/Vinod_Chopra_Films" title="Vinod Chopra Films">Vinod Chopra Films</a> <br/> <a href="/wiki/Rajkumar_Hirani" title="Rajkumar Hirani">Rajkumar Hirani Films</a>
</td>
<td>Hindi
</td>
<td><span data-sort-value="140 !"></span> <span style="white-space: nowrap">₹832 crore</span> (<span style="white-space: nowrap">US$120 million</span>)
</td>
<td><sup class="reference" id="cite_ref-pk_22-0"><a href="#cite_note-pk-22">[20]</a></sup><sup class="reference" id="cite_ref-statista_10-2"><a href="#cite_note-statista-10">[10]</a></sup>
</td></tr>
<tr>
<td>6
</td>
<td>5
</td>
<td><i><a href="/wiki/2.0_(film)" title="2.0 (film)">2.0</a></i>
</td>
<td>2018
</td>
<td><a class="mw-redirect" href="/wiki/S.Shankar" title="S.Shankar">S.Shankar</a>
</td>
<td><a href="/wiki/Lyca_Productions" title="Lyca Productions">Lyca Productions</a>
</td>
<td>Tamil
</td>
<td><span data-sort-value="116 !"></span> <span style="white-space: nowrap">₹800 crore</span> (<span style="white-space: nowrap">US$123 million</span>)
</td>
<td><sup class="reference" id="cite_ref-Box_office_day50_23-0"><a href="#cite_note-Box_office_day50-23">[21]</a></sup>
</td></tr>
<tr>
<td>7
</td>
<td>2
</td>
<td><i><a href="/wiki/Baahubali:_The_Beginning" title="Baahubali: The Beginning">Baahubali: The Beginning</a></i>
</td>
<td>2015
</td>
<td>S. S. Rajamouli
</td>
<td>Arka Media Works
</td>
<td>Telugu
</td>
<td><span data-sort-value="101 !"></span> <span style="white-space: nowrap">₹650 crore</span> (<span style="white-space: nowrap">US$101 million</span>)
</td>
<td><sup class="reference" id="cite_ref-baahubali_24-0"><a href="#cite_note-baahubali-24">[22]</a></sup><sup class="reference" id="cite_ref-augfirst_25-0"><a href="#cite_note-augfirst-25">[23]</a></sup><sup class="reference" id="cite_ref-26"><a href="#cite_note-26">[24]</a></sup>
</td></tr>
<tr>
<td>8
</td>
<td>4
</td>
<td><i><a href="/wiki/Sultan_(2016_film)" title="Sultan (2016 film)">Sultan</a></i>
</td>
<td>2016
</td>
<td><a href="/wiki/Ali_Abbas_Zafar" title="Ali Abbas Zafar">Ali Abbas Zafar</a>
</td>
<td><a href="/wiki/Yash_Raj_Films" title="Yash Raj Films">Yash Raj Films</a>
</td>
<td>Hindi
</td>
<td><span data-sort-value="096 !"></span> <span style="white-space: nowrap">₹623.33 crore</span> (<span style="white-space: nowrap">US$96 million</span>)
</td>
<td><sup class="reference" id="cite_ref-sultan_27-0"><a href="#cite_note-sultan-27">[25]</a></sup>
</td></tr>
<tr>
<td>9
</td>
<td>8
</td>
<td><i><a href="/wiki/Sanju" title="Sanju">Sanju</a></i>
</td>
<td>2018
</td>
<td>Rajkumar Hirani
</td>
<td>Rajkumar Hirani Films<br/>Vinod Chopra Films
</td>
<td>Hindi
</td>
<td><span data-sort-value="090.12 !"></span> <span style="white-space: nowrap">₹586.85 crore</span> (<span style="white-space: nowrap">US$90.12 million</span>)
</td>
<td><sup class="reference" id="cite_ref-Sanju_28-0"><a href="#cite_note-Sanju-28">[26]</a></sup>
</td></tr>
<tr>
<td>10
</td>
<td>7
</td>
<td><i><a href="/wiki/Padmaavat" title="Padmaavat">Padmaavat</a></i>
</td>
<td>2018
</td>
<td><a href="/wiki/Sanjay_Leela_Bhansali" title="Sanjay Leela Bhansali">Sanjay Leela Bhansali</a>
</td>
<td>Bhansali Productions <br/><a class="mw-redirect" href="/wiki/Viacom_18_Motion_Pictures" title="Viacom 18 Motion Pictures">Viacom 18 Motion Pictures</a>
</td>
<td>Hindi
</td>
<td><span data-sort-value="081 !"></span> <span style="white-space: nowrap">₹585 crore</span> (<span style="white-space: nowrap">US$90 million</span>)
</td>
<td><sup class="reference" id="cite_ref-:0_29-0"><a href="#cite_note-:0-29">[27]</a></sup><sup class="reference" id="cite_ref-statista_10-3"><a href="#cite_note-statista-10">[10]</a></sup>
</td></tr>
<tr>
<td>11
</td>
<td>8
</td>
<td><i><a href="/wiki/Tiger_Zinda_Hai" title="Tiger Zinda Hai">Tiger Zinda Hai</a></i>
</td>
<td>2017
</td>
<td><a href="/wiki/Ali_Abbas_Zafar" title="Ali Abbas Zafar">Ali Abbas Zafar</a>
</td>
<td>Yash Raj Films
</td>
<td>Hindi
</td>
<td><span data-sort-value="087.32 !"></span> <span style="white-space: nowrap">₹565.1 crore</span> (<span style="white-space: nowrap">US$87.32 million</span>)
</td>
<td><sup class="reference" id="cite_ref-boi-worldwide_9-2"><a href="#cite_note-boi-worldwide-9">[9]</a></sup><sup class="reference" id="cite_ref-tzh_30-0"><a href="#cite_note-tzh-30">[28]</a></sup>
</td></tr>
<tr>
<td>12
</td>
<td>1
</td>
<td><i><a href="/wiki/Dhoom_3" title="Dhoom 3">Dhoom 3</a></i>
</td>
<td>2013
</td>
<td><a href="/wiki/Vijay_Krishna_Acharya" title="Vijay Krishna Acharya">Vijay Krishna Acharya</a>
</td>
<td>Yash Raj Films
</td>
<td>Hindi
</td>
<td><span data-sort-value="101 !"></span> <span style="white-space: nowrap">₹556 crore</span> (<span style="white-space: nowrap">US$101 million</span>)
</td>
<td><sup class="reference" id="cite_ref-Dhoom3_37-0"><a href="#cite_note-Dhoom3-37">[n 3]</a></sup>
</td></tr>
<tr>
<td>13
</td>
<td>9
</td>
<td><i><a href="/wiki/War_(2019_film)" title="War (2019 film)">War</a></i>
</td>
<td>2019
</td>
<td><a href="/wiki/Siddharth_Anand" title="Siddharth Anand">Siddharth Anand</a>
</td>
<td>Yash Raj Films
</td>
<td>Hindi
</td>
<td><span data-sort-value="090 !"></span> <span class="nowrap"><span style="white-space: nowrap">₹</span>475.5 crore</span> (US$67 million)
</td>
<td><sup class="reference" id="cite_ref-hr_38-0"><a href="#cite_note-hr-38">[35]</a></sup>
</td></tr>
<tr>
<td>14
</td>
<td>1
</td>
<td><i><a href="/wiki/3_Idiots" title="3 Idiots">3 Idiots</a></i>
</td>
<td>2009
</td>
<td>Rajkumar Hirani
</td>
<td>Vinod Chopra Films
</td>
<td>Hindi
</td>
<td><span data-sort-value="090 !"></span> <span style="white-space: nowrap">₹460 crore</span> (<span style="white-space: nowrap">US$88 million</span>)
</td>
<td><sup class="reference" id="cite_ref-:1_31-1"><a href="#cite_note-:1-31">[29]</a></sup><sup class="reference" id="cite_ref-statista_10-5"><a href="#cite_note-statista-10">[10]</a></sup>
</td></tr>
<tr>
<td>15
</td>
<td>14
</td>
<td><i><a href="/wiki/Andhadhun" title="Andhadhun">Andhadhun</a></i>
</td>
<td>2018
</td>
<td><a href="/wiki/Sriram_Raghavan" title="Sriram Raghavan">Sriram Raghavan</a>
</td>
<td><a class="mw-redirect" href="/wiki/Viacom_18_Motion_Pictures" title="Viacom 18 Motion Pictures">Viacom 18 Motion Pictures</a> <br/>Matchbox Pictures
</td>
<td>Hindi
</td>
<td><span data-sort-value="066 !"></span> <span class="nowrap"><span style="white-space: nowrap">₹</span>456.89 crore</span> (US$64 million)
</td>
<td><sup class="reference" id="cite_ref-andhadhun_39-0"><a href="#cite_note-andhadhun-39">[36]</a></sup>
</td></tr>
<tr>
<td>16
</td>
<td>16
</td>
<td><i><a href="/wiki/Saaho" title="Saaho">Saaho</a></i>
</td>
<td>2019
</td>
<td><a href="/wiki/Sujeeth" title="Sujeeth">Sujeeth</a>
</td>
<td><a href="/wiki/UV_Creations" title="UV Creations">UV Creations</a> <br/> <a href="/wiki/T-Series_(company)" title="T-Series (company)">T-Series</a>
</td>
<td><a href="/wiki/Telugu_cinema" title="Telugu cinema">Telugu</a>
</td>
<td><span data-sort-value="061 !"></span> <span style="white-space: nowrap">₹433.06 crore</span> (<span style="white-space: nowrap">US$61 million</span>)
</td>
<td><sup class="reference" id="cite_ref-boxofficeindia.com_40-0"><a href="#cite_note-boxofficeindia.com-40">[37]</a></sup>
</td></tr>
<tr>
<td>17
</td>
<td>6
</td>
<td><i><a href="/wiki/Prem_Ratan_Dhan_Payo" title="Prem Ratan Dhan Payo">Prem Ratan Dhan Payo</a></i>
</td>
<td>2015
</td>
<td><a class="mw-redirect" href="/wiki/Sooraj_R._Barjatya" title="Sooraj R. Barjatya">Sooraj R. Barjatya</a>
</td>
<td><a href="/wiki/Fox_Star_Studios" title="Fox Star Studios">Fox Star Studios</a> <br/> <a href="/wiki/Rajshri_Productions" title="Rajshri Productions">Rajshri Productions</a>
</td>
<td>Hindi
</td>
<td><span data-sort-value="067 !"></span> <span style="white-space: nowrap">₹</span>432<span class="nowrap"> </span>crore (US$67<span class="nowrap"> </span>million)
</td>
<td><sup class="reference" id="cite_ref-dnaindia_41-0"><a href="#cite_note-dnaindia-41">[38]</a></sup><sup class="reference" id="cite_ref-statista_10-6"><a href="#cite_note-statista-10">[10]</a></sup>
</td></tr>
<tr>
<td>18
</td>
<td>2
</td>
<td><i><a href="/wiki/Chennai_Express" title="Chennai Express">Chennai Express</a></i>
</td>
<td>2013
</td>
<td><a href="/wiki/Rohit_Shetty" title="Rohit Shetty">Rohit Shetty</a>
</td>
<td><a href="/wiki/Red_Chillies_Entertainment" title="Red Chillies Entertainment">Red Chillies Entertainment</a>
</td>
<td>Hindi
</td>
<td><span data-sort-value="072 !"></span> <span style="white-space: nowrap">₹</span>423<span class="nowrap"> </span>crore (US$72.19<span class="nowrap"> </span>million)
</td>
<td><sup class="reference" id="cite_ref-chennai_42-0"><a href="#cite_note-chennai-42">[39]</a></sup>
</td></tr>
<tr>
<td>19
</td>
<td>4
</td>
<td><i><a href="/wiki/Kick_(2014_film)" title="Kick (2014 film)">Kick</a></i>
</td>
<td>2014
</td>
<td><a href="/wiki/Sajid_Nadiadwala" title="Sajid Nadiadwala">Sajid Nadiadwala</a>
</td>
<td><a href="/wiki/Nadiadwala_Grandson_Entertainment" title="Nadiadwala Grandson Entertainment">Nadiadwala Grandson</a>
</td>
<td>Hindi
</td>
<td><span data-sort-value="066 !"></span> <span style="white-space: nowrap">₹</span>402<span class="nowrap"> </span>crore (US$66<span class="nowrap"> </span>million)
</td>
<td><sup class="reference" id="cite_ref-BHGross_43-0"><a href="#cite_note-BHGross-43">[40]</a></sup>
</td></tr>
<tr>
<td>20
</td>
<td>17
</td>
<td><i><a href="/wiki/Simmba" title="Simmba">Simmba</a></i>
</td>
<td>2018
</td>
<td><a href="/wiki/Rohit_Shetty" title="Rohit Shetty">Rohit Shetty</a>
</td>
<td><a href="/wiki/Reliance_Entertainment" title="Reliance Entertainment">Reliance Entertainment</a> <br/> <a href="/wiki/Dharma_Productions" title="Dharma Productions">Dharma Productions</a>
</td>
<td>Hindi
</td>
<td><span data-sort-value="061 !"></span> <span style="white-space: nowrap">₹400 crore</span> (<span style="white-space: nowrap">US$61 million</span>)
</td>
<td><sup class="reference" id="cite_ref-simmba_44-0"><a href="#cite_note-simmba-44">[41]</a></sup>
</td></tr>
<tr>
<td>21
</td>
<td>5
</td>
<td><i><a href="/wiki/Happy_New_Year_(2014_film)" title="Happy New Year (2014 film)">Happy New Year</a></i>
</td>
<td>2014
</td>
<td><a href="/wiki/Farah_Khan" title="Farah Khan">Farah Khan</a>
</td>
<td>Red Chillies Entertainment
</td>
<td>Hindi
</td>
<td><span data-sort-value="065 !"></span> <span style="white-space: nowrap">₹397.21 crore</span> (<span style="white-space: nowrap">US$65.08 million</span>)
</td>
<td><sup class="reference" id="cite_ref-2014-gross_45-0"><a href="#cite_note-2014-gross-45">[42]</a></sup><sup class="reference" id="cite_ref-china-2015_46-0"><a href="#cite_note-china-2015-46">[43]</a></sup>
</td></tr>
<tr>
<td>22
</td>
<td>8
</td>
<td><i><a href="/wiki/Krrish_3" title="Krrish 3">Krrish 3</a></i>
</td>
<td>2013
</td>
<td><a href="/wiki/Rakesh_Roshan" title="Rakesh Roshan">Rakesh Roshan</a>
</td>
<td>Filmkraft Productions Pvt. Ltd
</td>
<td>Hindi
</td>
<td><span style="white-space: nowrap">₹393.37 crore</span> (<span style="white-space: nowrap">US$61 million</span>)
</td>
<td><sup class="reference" id="cite_ref-Hungam_47-0"><a href="#cite_note-Hungam-47">[44]</a></sup>
</td></tr>
<tr>
<td>23
</td>
<td>10
</td>
<td><i><a href="/wiki/Dilwale_(2015_film)" title="Dilwale (2015 film)">Dilwale</a></i>
</td>
<td>2015
</td>
<td>Rohit Shetty
</td>
<td>Red Chillies Entertainment <br/><a href="/wiki/Rohit_Shetty" title="Rohit Shetty">Rohit Shetty Productions</a>
</td>
<td>Hindi
</td>
<td><span style="white-space: nowrap">₹</span>376.85 (<span style="white-space: nowrap">US$58 million</span>)
</td>
<td><sup class="reference" id="cite_ref-48"><a href="#cite_note-48">[45]</a></sup>
</td></tr>
<tr>
<td>24
</td>
<td>21
</td>
<td><i><a href="/wiki/Kabir_Singh" title="Kabir Singh">Kabir Singh</a></i>
</td>
<td>2019
</td>
<td><a href="/wiki/Sandeep_Vanga" title="Sandeep Vanga">Sandeep Vanga</a>
</td>
<td>Cine1 Studios <br/> <a href="/wiki/T-Series_(company)" title="T-Series (company)">T-Series</a>
</td>
<td>Hindi
</td>
<td><span style="white-space: nowrap">₹367.68 crore</span> (<span style="white-space: nowrap">US$56 million</span>)
</td>
<td><sup class="reference" id="cite_ref-KS_bo_total_49-0"><a href="#cite_note-KS_bo_total-49">[46]</a></sup>
</td></tr>
<tr>
<td>25
</td>
<td>25
</td>
<td><i><a href="/wiki/Tanhaji" title="Tanhaji">Tanhaji</a></i>
</td>
<td>2020
</td>
<td><a href="/wiki/Om_Raut" title="Om Raut">Om Raut</a>
</td>
<td><a href="/wiki/Ajay_Devgn_FFilms" title="Ajay Devgn FFilms">Ajay Devgn FFilms</a><br/><a href="/wiki/T-Series_(company)" title="T-Series (company)">T-Series</a>
</td>
<td>Hindi
</td>
<td><span style="white-space: nowrap">₹366.36 crore</span> (<span style="white-space: nowrap">US$56 million</span>)
</td>
<td><sup class="reference" id="cite_ref-tanhaji-bo_50-0"><a href="#cite_note-tanhaji-bo-50">[47]</a></sup>
</td></tr></tbody></table>
header = [th.text.rstrip() for th in rows[0].find_all('th')]
print(header)
print('------------')
print(len(header))
---------------------------------------------------------------------------
NameError                                 Traceback (most recent call last)
<ipython-input-59-c561ea238c21> in <module>
----> 1 header = [th.text.rstrip() for th in rows[0].find_all('th')]
      2 print(header)
      3 print('------------')
      4 print(len(header))

NameError: name 'rows' is not defined
rows = my_table.findAll("tr")
len(rows)
26
my_table_data = []
for row in rows[1:]:
            data = [d.text.rstrip() for d in row.select('td')]
            my_table_data.append(data)
my_table_data[0:2]
[['1',
  '1',
  'Dangal',
  '2016',
  'Nitesh Tiwari',
  'Aamir Khan Productions  UTV Motion Pictures  Walt Disney Studios India',
  'Hindi',
  ' ₹2,024 crore (US$311 million)',
  '[9]'],
 ['2',
  '1',
  'Baahubali 2: The Conclusion',
  '2017',
  'S. S. Rajamouli',
  'Arka Media Works',
  'Telugu',
  ' ₹1,810 crore (US$278 million)',
  '[9]']]
df = pd.DataFrame(my_table_data)
df.head()
0	1	2	3	4	5	6	7	8
0	1	1	Dangal	2016	Nitesh Tiwari	Aamir Khan Productions UTV Motion Pictures W...	Hindi	₹2,024 crore (US$311 million)	[9]
1	2	1	Baahubali 2: The Conclusion	2017	S. S. Rajamouli	Arka Media Works	Telugu	₹1,810 crore (US$278 million)	[9]
2	3	3	Bajrangi Bhaijaan	2015	Kabir Khan	Salman Khan Films Kabir Khan Films Eros Inte...	Hindi	₹969.06 crore (US$150 million)	[n 1]
3	4	3	Secret Superstar	2017	Advait Chandan	Aamir Khan Productions	Hindi	₹966.86 crore (US$154 million)	[n 2]
4	5	1	PK	2014	Rajkumar Hirani	Vinod Chopra Films Rajkumar Hirani Films	Hindi	₹832 crore (US$120 million)	[20][10]
Cleaning
df.rename(columns = {0:"Rank"}, inplace = True)
df.rename(columns = {1:"Peak"}, inplace = True)
df.rename(columns = {2:"Film"}, inplace = True)
df.rename(columns = {3:"Year"}, inplace = True)
df.rename(columns = {4:"Director"}, inplace = True)
df.rename(columns = {5:"Producer"}, inplace = True)
df.rename(columns = {6:"Language"}, inplace = True)
df.rename(columns = {7:"Worldwide_Gross_INR"}, inplace = True)
df = df.drop(columns=[8])


df.head()
Rank	Peak	Film	Year	Director	Producer	Language	Worldwide_Gross_INR
0	1	1	Dangal	2016	Nitesh Tiwari	Aamir Khan Productions UTV Motion Pictures W...	Hindi	₹2,024 crore (US$311 million)
1	2	1	Baahubali 2: The Conclusion	2017	S. S. Rajamouli	Arka Media Works	Telugu	₹1,810 crore (US$278 million)
2	3	3	Bajrangi Bhaijaan	2015	Kabir Khan	Salman Khan Films Kabir Khan Films Eros Inte...	Hindi	₹969.06 crore (US$150 million)
3	4	3	Secret Superstar	2017	Advait Chandan	Aamir Khan Productions	Hindi	₹966.86 crore (US$154 million)
4	5	1	PK	2014	Rajkumar Hirani	Vinod Chopra Films Rajkumar Hirani Films	Hindi	₹832 crore (US$120 million)
df.to_excel('Bollywood_movies1.xlsx', index=True)


 




 
