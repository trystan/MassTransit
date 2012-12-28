Logging in MassTransit
======================

Logging in MassTransit is done with the de facto logging tool 'log4net.' This tool was chosen for its many years of battle hardened code and ease of use. You can find out more about log4net and its configuration at http://logging.apache.org/log4net

Like NHibernate's 'NHibernate.SQL' where all of NHibernate's generated sql is logged, MassTransit has a log named 'MassTransit.Messages' where all of the message traffic is logged. This logging looks like:

    RECV:{Address}:{Message Id}:{Message Type Name}
    SEND:{Address}:{Message Name}

Logging with MassTransit.Log4net
''''''''''''''''''''''

First you have to get MassTransit.Log4Net. You can use Nuget or download it from https://nuget.org/packages/MassTransit.Log4Net.

Then configure MassTransit to use Log4Net during the service bus initialization.

.. sourcecode:: csharp

    using MassTransit.Log4NetIntegration;
    

    XmlConfigurator.Configure();

    var bus = ServiceBusFactory.New(sbc =>
        {
            /* usual stuff */
            sbc.UseLog4Net();
        });

The easiest way to configure log4net is is to add the configuration to the web.config or app.config file.

.. sourcecode:: xml

      <configSections>
        <section name="log4net" type="log4net.Config.Log4NetConfigurationSectionHandler, log4net" />
      </configSections>
      <log4net>
        <appender name="RollingFile" type="log4net.Appender.RollingFileAppender">
          <file value="MassTransit.log" />
          <appendToFile value="true" />
          <maximumFileSize value="1000KB" />
          <maxSizeRollBackups value="5" />
    
          <layout type="log4net.Layout.PatternLayout">
            <conversionPattern value="%level %thread %logger - %message%newline" />
          </layout>
        </appender>
    
        <root>
          <level value="DEBUG" />
          <appender-ref ref="RollingFile" />
        </root>
      </log4net>

This will create a MassTransit.log file in the root folder. There are a lot of configuration options explained at http://logging.apache.org/log4net/release/manual/configuration.html.
