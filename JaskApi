from errbot import BotPlugin, botcmd, arg_botcmd, webhook
import requests
import json

api_key = ''
user_name = ''
url = 'https://apex.portal.jask.ai/api/v1/'


class Jaskapi(BotPlugin):
    """
    Connection to JaskApi
    """
    # Passing split_args_with=None will cause arguments to be split on any kind
    # of whitespace, just like Python's split() does

    @arg_botcmd('query', type=str)
    def insights(self, mess, query=None):
        base_url = 'https://apex.portal.jask.ai/api/v1/insights?q='

        full_url = '%s%s&offset=0&limit=1' % (base_url, query)

        response = requests.get(full_url, params={'username': user_name, 'api_key': api_key})
        
        response_json = response.json()
        
        objects = response_json["data"]["objects"][0]

        return objects['entity']['ip']['address']


    @arg_botcmd('query', type=str)
    def signal(self, mess, query=None):
        base_url = 'https://apex.portal.jask.ai/api/v1/insights?q='

        full_url = '%s%s&offset=0&limit=1' % (base_url, query)

        response = requests.get(full_url, params={'username': user_name, 'api_key': api_key})
        
        response_json = response.json()
        
        signals = response_json["data"]["objects"][0]['signals']

        length = len(signals)
        insight_signals = {}
        for i in range(length):
            insight_signals[signals[i]['id']] = signals[i]['name']

        return insight_signals

        #return signals[0]['name']

    @arg_botcmd('insight', type=str)
    @arg_botcmd('--assignee', type=str, dest='assignee')
    def assignto(self, mess, insight=None, assignee=None):
        base_url = 'https://apex.portal.jask.ai/api/v1/insights/'

        headers = {'content-type': 'application/json'}

        requestBody = {'assignee': {'type': 'USER', 'value': assignee}}

        full_url = '%s%s/assignee' % (base_url, insight)

        jsonBody = json.dumps(requestBody)
        
        response = requests.put(full_url, params={'username': user_name, 'api_key': api_key}, data=jsonBody, headers=headers, verify=False)
        
        if response.status_code != 200:
            return response.status_code
        else:
            return '%s has been assigned to insight %s' % (assignee, insight)

    @arg_botcmd('insight', type=str)
    @arg_botcmd('--status', type=str, dest='status')
    def instatus(self, mess, insight=None, status=None):
        base_url = 'https://apex.portal.jask.ai/api/v1/insights/'

        headers = {'content-type': 'application/json'}
        requestBody = {'status': status}

        full_url = '%s%s/status' % (base_url, insight)

        jsonBody = json.dumps(requestBody)

        response = requests.put(full_url, params={'username': user_name, 'api_key': api_key}, data=jsonBody, headers=headers, verify=False)

        if response.status_code != 200:
            return 'There was an %s error.'
        else:
            return 'Insight %s status has been updated to %s' % (insight, status)

        
