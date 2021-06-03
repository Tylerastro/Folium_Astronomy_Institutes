# Folium Astronomy Institutes
.py file creates map but cannot show. Use ipynb file in Jupyter to see the map.
![image](https://github.com/Tylerastro/Folium_Astronomy_Institutes/blob/main/Cover.png)

## Note
Not all institutes are included. Any helpful PR is appreciated and welcomed.

## 注意
並非全部的研究所都有被包含。歡迎任何PR，可以讓這地圖更加完整。

## Description
This python-based code produces html file, aiming to mark astronomy institutes on the world map to help students to find their future place. It is designed to select research area, so people can find their interests directly.
For more folium usage, please refer to https://github.com/python-visualization/folium

## 內容
這個Python code主要是要幫助學生或者博士生，快速視覺化的找國內外天文所。檔案設計有研究領域的分類，可以快速尋找有興趣的領域。
地圖主要使用Folium套件，詳細內容請參照 https://github.com/python-visualization/folium

## Usage
You can open map.html file directly, which is a ready to use file.

Switching to different area or subjects or even more informations are feasible.
* Edit input csv file (Adding more info and change major)
* Change map clusters
* Adding/Deleting Groups

### Edit input csv file
The headers in our csv file are 
* Name
* URL
* Building_Location (From official website)
* Location (From Google map)
* Region
* Independency (If it's an independent institute)
* Job_opportunities (Link to job vacancy)
* Colloquiums (Public talks, if they do have) Note: it is not shown in the map
* OCW (Open Course Ware, if they do have) Note: it is not shown in the map
* latitude
* longitude


### Change map clusters
In default setting, the institutes are grouped. If you want to disable this, comment out
```
mcg = folium.plugins.MarkerCluster(control=False)
map_institute.add_child(mcg)
```
This will disable the clustering.
And change the `transient = folium.plugins.FeatureGroupSubGroup(mcg, "Transients",show=False)` to `transient = folium.FeatureGroup( "Transients",show=False)` since we diable the main group, now each group are the main FeatureGroup.

### Adding/Deleting Groups
The catalogs function is defined in the code:
```
def catalogs(keyword,catalogue): # key word for boolean institutes ['keyword'], catalogue is the research area name
    for index, item in data.iterrows():
        if isNaN(item['Region']):
            continue
        else:
            keys = re.findall(r"[\w']+", item['Region'])
            keys = [word.title() for word in keys]
            location = [item['latitude'], item['longitude']]
            html = ('<a href='+str(item['URL'])+ ' target="_blank">' +str(item['Name'])+'</a>'+'<br>'+
                '<a href='+str(item['Job_opportunities'])+ ' target="_blank">'+'Job Opportunity '+'</a>'+'<br>'
                +'<p>'+'Research area: '+'<br>'+str(item['Region'].title()).replace(',','<br>')+'</p>')
            iframe = folium.IFrame(html,
                               width=2200,
                               height=170)
            popup = folium.Popup(iframe,
                             max_width=300)
            if any(set(keyword)&set(keys)):
                folium.Marker(location, tooltip=str(item['Name'])+'<br>'+'Independency: '+str(item['Independency'])
                              ,popup = popup, icon=folium.Icon(icon='university', prefix='fa')).add_to(catalogue)

```
Simply usage is to copy the following template and paste on the main.py
```
interest_research_area  = folium.plugins.FeatureGroupSubGroup(mcg, "Option_Name_on_map",show=False)
map_institute.add_child(interest_research_area)
catalogs(['Key word list'],interest_research_area)
```
