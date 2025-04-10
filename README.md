
:no_entry: [DEPRECATED]

# ^^sure, but I'm going to make this work
followed this [comment on the original repo issue](https://github.com/shuaiscott/zap2xml/issues/14#issuecomment-2765029439)
but because something could happen to that I'll paste it here:
```
The image was pulling fine for me, but the script wasn't working because the zap2it service seems to have been shut down on 3/25. I found a comment on a news article that the URL simply changed to gracenote. So I did the following to get my kube container working again:

    Fork this repo
    Update main script line 158 to '$urlRoot = 'https://tvlistings.gracenote.com/';'
    Create a dockerhub repo (or wherever you want to host your image)
    Do a 'docker build -t user/repo .' followed by 'docker push user/repo'
    Update your source image to your newly pushed image

I didn't have to update any vars or anything, just that one liner seems to get everything working (for now anyway, hopefully it remains that way).
```
Thanks [bstock84](https://github.com/bstock84)

# my steps
1. change the urlRoot just as outlined in the comment
2. build locally, no need to push anywhere: `docker image build -t everydaycombat/zap2xml:latest .`
3. switch zap2xml docker run script in unraid repo to use my image


# zap2xml
Docker container for zap2xml

This is zap2xml with Environment Variables driving the configuration. By default it runs every 12 hours to update your EPG data from zap2it. This container will take a second account for zap2it and will merge the received xml files into one using tv_merge.

## Quick Run
`docker run -d --name zap2xml -v /xmltvdata:/data -e USERNAME=youremail@email.com -e PASSWORD=**password** -e OPT_ARGS="-I -D -a" -e USERNAME2=yourseconduser@email.com -e PASSWORD2=**secondpassword** -e OPT_ARGS2="-I -D" -e XMLTV_FILENAME=xmltv.xml shuaiscott/zap2xml`

## Environment Settings
You can configure the following environment variables below:

### Required
- USERNAME - zap2it.com username
- PASSWORD - zap2it.com password

### Optional
- OPT_ARGS - additional command line arguments for zap2xml
- USERNAME2 - Second zap2it.com username
- PASSWORD2 - Second zap2it.com password
- OPT_ARGS2 = additional command line arguments for zap2xml for the second username
- XMLTV_FILENAME - filename for your xmltv file (default: xmltv.xml)
- SLEEPTIME - time in seconds to wait before next run (default: 43200)
