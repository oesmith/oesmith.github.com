---
layout: post
title: RESTful Services in Visual C# 2008
---
Here’s a problem that had me scratching my head for a while this weekend. How
do I create a simple REST service using WCF (Windows Communication Foundation)?

MSDN has a [great little tutorial][1] that explains how to code the service and
get it running in a standalone application context. However, it fails to
address getting the service deployed on an IIS server.

The key point that seems to be missing from the documentation is that no matter
what you do with the web.config file, you’re never going to get a configuration
that works as a plain-old REST web service. Instead, you have to delete the web
service configuration completely from the web.config file, and add the
declaration `Factory="System.ServiceModel.Activation.WebServiceHostFactory"` to
the ServiceHost element in the .svc file associated with your service.

Here’s a quick and dirty example:

{% highlight csharp %}
<%@ ServiceHost Factory="System.ServiceModel.Activation.WebServiceHostFactory"
    Language="C#" Service="Service1" %>
using System.ServiceModel;
using System.ServiceModel.Channels;
using System.ServiceModel.Web;
using System.Runtime.Serialization;
[ServiceContract]
public class Service1
{
    [OperationContract]
    [WebGet(UriTemplate="{id}")]
    DataType GetData(string id)
    {
        return new DataType(1,"Foo");
    }
}
[DataContract(Name="data",Namespace="http://example.org")]
public class DataType
{
    public DataType(int id, string name)
    {
        this.id = id;
        this.name = name;
    }
    [DataMember]
    int id;
    [DataMember]
    string name;
}
{% endhighlight %}

Calling `http://host/Service1.svc/123` returns:

{% highlight xml %}
<data xmlns="http://example.org" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
    <id>1</id>
    <name>Foo</name>
</data>
{% endhighlight %}

That’s actually quite easy really, isn’t it? If only the MSDN docs told you
that..

[1]: http://msdn2.microsoft.com/en-us/library/bb412178.aspx
