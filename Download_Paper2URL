# coding: utf-8
import sys
import json
import os.path
from pathlib import Path

if (3,0) <= sys.version_info < (4,0):
    import http.client as httplib
elif (2,6) <= sys.version_info < (3,0):
    import httplib

TOKEN = "Bearer i_V4E1xwyyAAAAAAAAAF_EM8xJKR6FA79POyFGfl6DTsCLlbUDawFtCwqLs4vOYx"
PathInput = input('Input the Dropbox path you want to organize your Paper Doc:>')

def PaperDocOrg():
    headers = {
        "Authorization": TOKEN,
        "Content-Type": "application/json"
      }

    params = {
        "sort_order": {".tag":"descending"}
    }

    c = httplib.HTTPSConnection("api.dropboxapi.com")
    c.request("POST", "/2/paper/docs/list", json.dumps(params), headers)
    r = c.getresponse()

    Jsonlist = r.read()
    Diclist = json.loads(Jsonlist)['doc_ids']
    Cursorlist = json.loads(Jsonlist)['cursor']
    Cursor = Cursorlist['value']



    for k in Diclist:

        """Check if ListFile exist"""
        pathList = PathInput + '/**list.txt'
        my_ListFile = Path(pathList)

        if my_ListFile.exists():

            if k in open(pathList).read():
                pass
            else:
                print('Downloading Paper Docs...')
                IDRef = "{\"doc_id\":\"" + k + "\",\"export_format\":{\".tag\":\"markdown\"}}"
                # print(IDRef)
                headersDL = {
                    "Authorization": TOKEN,
                    "Dropbox-API-Arg": IDRef
                }

                c = httplib.HTTPSConnection("api.dropboxapi.com")
                c.request("POST", "/2/paper/docs/download", "", headersDL)
                r = c.getresponse()

                r01 = r.getheader('Dropbox-Api-Result')
                r02 = json.loads(r01)

                TITLE = r02['title'].replace(' ', '_').replace('/','_')

                DocTitle = '/' + TITLE

                path = PathInput + DocTitle + '.url'


                with open(path, mode='w') as f:
                    f.write(
                        '[InternetShortcut]' + '\n' +'URL=https://paper.dropbox.com/'+ k +'\n'
                    )

                with open(pathList, mode='a') as Listfile:
                    Listfile.write(k + '\n')


        else:
            print('Downloading Paper Docs...')
            IDRef = "{\"doc_id\":\"" + k + "\",\"export_format\":{\".tag\":\"markdown\"}}"
            # print(IDRef)
            headersDL = {
                "Authorization": TOKEN,
                "Dropbox-API-Arg": IDRef
            }

            c = httplib.HTTPSConnection("api.dropboxapi.com")
            c.request("POST", "/2/paper/docs/download", "", headersDL)
            r = c.getresponse()

            r01 = r.getheader('Dropbox-Api-Result')
            r02 = json.loads(r01)

            TITLE = r02['title'].replace(' ', '_').replace('/', '_')

            DocTitle = '/' + TITLE

            path = PathInput + DocTitle + '.url'

            with open(path, mode='w') as f:
                f.write(
                    '[InternetShortcut]' + '\n' + 'URL=https://paper.dropbox.com/' + k + '\n'
                )

            with open(pathList, mode='a') as Listfile:
                Listfile.write(k + '\n')



if __name__ == '__main__':
    PaperDocOrg()
