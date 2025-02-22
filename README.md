# dashboard-email-scheduler

A common request I have received from my customers is the ability to create performance reports to share with other members of their team or to their clients. While New Relic does provide email weekly performance reports, it is often the case that customers wish to email custom reports with the performance and business metrics they care about most.

With the New Relic NerdGraph API, it is now possible to retrieve the link for the current snapshot of your New Relic Dashboard! This opens up a lot of programmable capabilities for our customers. One use-case would be to programmatically generate a snapshot of a performance dashboard that could be mailed every week.

This project provides the ability to periodically share a New Relic Dashboard using the GMail API.

https://discuss.newrelic.com/t/periodically-share-a-new-relic-dashboard-using-the-gmail-api/108919

## Environment Setup 

Clone the [Github repository](https://github.com/AnthonyBloomer/dashboard-email-scheduler/) and set up a virtual environment.

```
git clone https://github.com/ahealy-newr/dashboard-email-scheduler.git
cd dashboard-email-scheduler
virtualenv env
source env/bin/activate
```

Install the project requirements.

``` shell
pip install -r requirements.txt
```

Export your [USER API Key](https://docs.newrelic.com/docs/apis/intro-apis/new-relic-api-keys/#user-key) as an environment variable.

``` shell
export NEW_RELIC_PERSONAL_API_KEY="YOUR_API_KEY"
```

If you have an EU based account, you will also need to export the `NEW_RELIC_REGION` environment variable:

``` shell
export NEW_RELIC_REGION="EU"
```

Edit `config.py` with the New Relic Dashboard and Email settings.

``` python
config = {
    "email": {
        "sender": "Email sender.",
        "to": "Email receiver.",
        "subject": "Email subject",
        "text": "Email message text",
    },
    "dashboard": {
        "guid": "The New Relic Dashboard GUID",
        "file_type": "PDF",
        "width": "2000",
        "height": "2000",
    },
}
```

## GMAIL API Setup

1. Go to the [Google Developer Console](https://console.developers.google.com/?pli=1) and create a new project. 
2. Select Enable APIS and Services and then select GMail API option and press ENABLE
3. Select Credentials on the left panel
4. Select Create Credentials and OAuth Client ID option
5. Application type -> Dekstop App
6. Name for device you will run the scheduler from
7. Select Create
8. Download JSON secret and dump to root directory of this project and rename to credentials.json

## Usage
Run `python scheduler.py`. This will run an initial job so you can authenticate the application. By default the job will run ever Friday. Refer to the [Schedule](https://pypi.org/project/schedule/) project documentation to configure the scheduler to your liking. 
