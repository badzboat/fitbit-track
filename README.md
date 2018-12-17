## fitbit-track-Python  (reference)
https://towardsdatascience.com/collect-your-own-fitbit-data-with-python-ff145fa10873

## Step
 1) Add app on Fitbit Developer.com
 2) Clone unofficial fitbit =>  git clone https://github.com/orcasgit/python-fitbit
 3) Get Client_ID and Client_Secret from Fitbit Developer add app page
 4) Write code
 ![image](https://user-images.githubusercontent.com/16419246/50111959-9b968080-0203-11e9-84de-95ba7f0fa248.png)
 ![image](https://user-images.githubusercontent.com/16419246/50111997-c385e400-0203-11e9-8960-94759a71716c.png)
![image](https://user-images.githubusercontent.com/16419246/50112075-f62fdc80-0203-11e9-82f5-7a6dfd6fef58.png)


import fitbit
import gather_keys_oauth2 as Oauth2
import pandas as pd 
import datetime
CLIENT_ID = '22CPDQ'   (CHANGE TO OUR CLIENT ID)
CLIENT_SECRET = '56662aa8bf31823e4137dfbf48a1b5f1'    (CHANGE TO OUR CLIENT SECRET)

server = Oauth2.OAuth2Server(CLIENT_ID, CLIENT_SECRET)
server.browser_authorize()
# Will browse IE to Fitbit authentication page and allow services



# Ready to access fitbit data (Read token)
ACCESS_TOKEN = str(server.fitbit.client.session.token['access_token'])
REFRESH_TOKEN = str(server.fitbit.client.session.token['refresh_token'])
auth2_client = fitbit.Fitbit(CLIENT_ID, CLIENT_SECRET, oauth2=True, access_token=ACCESS_TOKEN, refresh_token=REFRESH_TOKEN)


# Create date time format
yesterday = str((datetime.datetime.now() - datetime.timedelta(days=1)).strftime("%Y%m%d"))
yesterday2 = str((datetime.datetime.now() - datetime.timedelta(days=1)).strftime("%Y-%m-%d"))
today = str(datetime.datetime.now().strftime("%Y%m%d"))


# Start to pull data
fit_statsHR = auth2_client.intraday_time_series('activities/heart', base_date=yesterday2, detail_level='1sec')

time_list = []
val_list = []
for i in fit_statsHR['activities-heart-intraday']['dataset']:
    val_list.append(i['value'])
    time_list.append(i['time'])
heartdf = pd.DataFrame({'Heart Rate':val_list,'Time':time_list})

# Export to CSV daily file
heartdf.to_csv('/Users/shsu/Downloads/python-fitbit-master/Heart/heart'+ \
               yesterday+'.csv', \
               columns=['Time','Heart Rate'], header=True, \
               index = False)
               
               
               

# Follow page: https://towardsdatascience.com/collect-your-own-fitbit-data-with-python-ff145fa10873
# Source:  https://github.com/orcasgit/python-fitbit
