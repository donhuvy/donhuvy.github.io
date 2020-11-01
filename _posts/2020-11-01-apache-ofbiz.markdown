---
layout: post
title:  "Create new plugin in Apache Ofbiz 17.12.04"
date:   2020-11-01 22:30:00 +0700
categories: ofbiz java apache-ofbiz
---
Create new plugin in Apache Ofbiz 17.12.04

```
git clone https://gitbox.apache.org/repos/asf/ofbiz-framework.git ofbiz-framework
gradlew cleanAll loadDefault
gradle ofbiz
```

![result](/images/2020_11_01_ofbiz_plugin.png)

```
gradlew createPlugin -PpluginId=ofbizDemo
gradlew loadAll # it is not loadDefault
gradlew ofbiz
```

File `OfbizDemoUiLabels.xml`

```xml
<resource xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="http://ofbiz.apache.org/dtds/ofbiz-properties.xsd">
    <property key="OfbizDemoApplication">
        <value xml:lang="en">OfbizDemo Application</value>
    </property>
    <property key="OfbizDemoCompanyName">
        <value xml:lang="en">OFBiz: OfbizDemo</value>
    </property>
    <property key="OfbizDemoCompanySubtitle">
        <value xml:lang="en">Part of the Apache OFBiz Family of Open Source Software</value>
    </property>
    <property key="OfbizDemoViewPermissionError">
        <value xml:lang="en">You are not allowed to view this page.</value>
    </property>
    
	<property key="OfbizDemoFind">
	    <value xml:lang="en">Find</value>
	</property>
	<property key="OfbizDemoFirstName">
	    <value xml:lang="en">First Name</value>
	</property>
	<property key="OfbizDemoId">
	    <value xml:lang="en">OFBiz Demo Id</value>
	</property>
	<property key="OfbizDemoLastName">
	   <value xml:lang="en">Last Name</value>
	</property>
</resource>
```

File `OfbizDemoDemoData.xml`

```xml
<entity-engine-xml>
    <OfbizDemo ofbizDemoId="SAMPLE_DEMO_1" ofbizDemoTypeId="INTERNAL" firstName="Sample First 1" lastName="Sample Last 1" comments="This is test comment for first record."/>
    <OfbizDemo ofbizDemoId="SAMPLE_DEMO_2" ofbizDemoTypeId="INTERNAL" firstName="Sample First 2" lastName="Sample last 2" comments="This is test comment for second record."/>
    <OfbizDemo ofbizDemoId="SAMPLE_DEMO_3" ofbizDemoTypeId="EXTERNAL" firstName="Sample First 3" lastName="Sample last 3" comments="This is test comment for third record."/>
    <OfbizDemo ofbizDemoId="SAMPLE_DEMO_4" ofbizDemoTypeId="EXTERNAL" firstName="Sample First 4" lastName="Sample last 4" comments="This is test comment for fourth record."/>
</entity-engine-xml>
```

File `OfbizDemoSecurityGroupDemoData.xml`

```xml
<entity-engine-xml>
    <SecurityGroupPermission fromDate="2001-05-13 12:00:00.0" groupId="FULLADMIN" permissionId="OFBIZDEMO_ADMIN"/>
    <SecurityGroupPermission fromDate="2001-05-13 12:00:00.0" groupId="FLEXADMIN" permissionId="OFBIZDEMO_CREATE"/>
    <SecurityGroupPermission fromDate="2001-05-13 12:00:00.0" groupId="FLEXADMIN" permissionId="OFBIZDEMO_DELETE"/>
    <SecurityGroupPermission fromDate="2001-05-13 12:00:00.0" groupId="FLEXADMIN" permissionId="OFBIZDEMO_UPDATE"/>
    <SecurityGroupPermission fromDate="2001-05-13 12:00:00.0" groupId="FLEXADMIN" permissionId="OFBIZDEMO_VIEW"/>
    <SecurityGroupPermission fromDate="2001-05-13 12:00:00.0" groupId="VIEWADMIN" permissionId="OFBIZDEMO_VIEW"/>
    <SecurityGroupPermission fromDate="2001-05-13 12:00:00.0" groupId="BIZADMIN" permissionId="OFBIZDEMO_ADMIN"/>
</entity-engine-xml>
```

File `OfbizDemoSecurityPermissionSeedData.xml`

```xml
<entity-engine-xml>
    <SecurityPermission description="View operations in the OfbizDemo Component." permissionId="OFBIZDEMO_VIEW"/>
    <SecurityPermission description="Create operations in the OfbizDemo Component." permissionId="OFBIZDEMO_CREATE"/>
    <SecurityPermission description="Update operations in the OfbizDemo Component." permissionId="OFBIZDEMO_UPDATE"/>
    <SecurityPermission description="Delete operations in the OfbizDemo Component." permissionId="OFBIZDEMO_DELETE"/>
    <SecurityPermission description="ALL operations in the OfbizDemo Component." permissionId="OFBIZDEMO_ADMIN"/>

    <SecurityGroupPermission fromDate="2001-05-13 12:00:00.0" groupId="SUPER" permissionId="OFBIZDEMO_ADMIN"/>

</entity-engine-xml>
```

File `OfbizDemoTypeData.xml`

```XML
<entity-engine-xml>
    <OfbizDemoType ofbizDemoTypeId="INTERNAL" description="Internal Demo - Office"/>
    <OfbizDemoType ofbizDemoTypeId="EXTERNAL" description="External Demo - On Site"/>
</entity-engine-xml>
```

File `OfbizDemoServices.groovy`

```xml
import org.apache.ofbiz.entity.GenericEntityException;

def createOfbizDemo() {

    println("VY : ........");
    result = [:];
    try {
        ofbizDemo = delegator.makeValue("OfbizDemo");
        // Auto generating next sequence of ofbizDemoId primary key
        ofbizDemo.setNextSeqId();
        // Setting up all non primary key field values from context map
        ofbizDemo.setNonPKFields(context);
        // Creating record in database for OfbizDemo entity for prepared value
        ofbizDemo = delegator.create(ofbizDemo);
        result.ofbizDemoId = ofbizDemo.ofbizDemoId;
        logInfo("==========This is my first Groovy Service implementation in Apache OFBiz. OfbizDemo record "
                +"created successfully with ofbizDemoId: "+ofbizDemo.getString("ofbizDemoId"));
    } catch (GenericEntityException e) {
        logError(e.getMessage());
        return error("Error in creating record in OfbizDemo entity ........");
    }
    return result;
}
```

File `services.xml`

```xml
<services xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:noNamespaceSchemaLocation="http://ofbiz.apache.org/dtds/services.xsd">
	<description>OfbizDemo Services</description>
	<vendor></vendor>
	<version>1.0</version>

	
	<service name="createOfbizDemo" default-entity-name="OfbizDemo" engine="entity-auto" invoke="create" auth="true">
		<description>Create an Ofbiz Demo record</description>
		<auto-attributes include="pk" mode="OUT" optional="false" />
		<auto-attributes include="nonpk" mode="IN" optional="false" />
		<override name="comments" optional="true" />
	</service>

	<service name="createOfbizDemoByGroovyService" default-entity-name="OfbizDemo" engine="groovy"
			 location="component://ofbizDemo/groovyScripts/com/companyname/ofbizdemo/OfbizDemoServices.groovy" invoke="createOfbizDemo" auth="true">
		<description>Create an Ofbiz Demo record using a service in Groovy</description>
		<auto-attributes include="pk" mode="OUT" optional="false"/>
		<auto-attributes include="nonpk" mode="IN" optional="false"/>
		<override name="comments" optional="true"/>
	</service>

</services>
```

File `OfbizDemoEvents.java`

```java
package com.companyname.ofbizdemo.events;

import org.apache.ofbiz.base.util.Debug;
import org.apache.ofbiz.base.util.UtilMisc;
import org.apache.ofbiz.base.util.UtilValidate;
import org.apache.ofbiz.entity.Delegator;
import org.apache.ofbiz.entity.GenericValue;
import org.apache.ofbiz.service.GenericServiceException;
import org.apache.ofbiz.service.LocalDispatcher;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class OfbizDemoEvents {

    public static final String module = OfbizDemoEvents.class.getName();

    public static String createOfbizDemoEvent(HttpServletRequest request, HttpServletResponse response) {
        Delegator delegator = (Delegator) request.getAttribute("delegator");
        LocalDispatcher dispatcher = (LocalDispatcher) request.getAttribute("dispatcher");
        GenericValue userLogin = (GenericValue) request.getSession().getAttribute("userLogin");
        String ofbizDemoTypeId = request.getParameter("ofbizDemoTypeId");
        String firstName = request.getParameter("firstName");
        String lastName = request.getParameter("lastName");
        if (UtilValidate.isEmpty(firstName) || UtilValidate.isEmpty(lastName)) {
            String errMsg = "First Name and Last Name are required fields on the form and can't be empty.";
            request.setAttribute("_ERROR_MESSAGE_", errMsg);
            return "error";
        }
        String comments = request.getParameter("comments");
        try {
            Debug.logInfo("=======Creating OfbizDemo record in event using service createOfbizDemoByGroovyService=========", module);
            dispatcher.runSync("createOfbizDemoByGroovyService", UtilMisc.toMap("ofbizDemoTypeId", ofbizDemoTypeId,
                    "firstName", firstName, "lastName", lastName, "comments", comments, "userLogin", userLogin));
        } catch (GenericServiceException e) {
            String errMsg = "Unable to create new records in OfbizDemo entity: " + e.toString();
            request.setAttribute("_ERROR_MESSAGE_", errMsg);
            return "error";
        }
        request.setAttribute("_EVENT_MESSAGE_", "OFBiz Demo created succesfully.");
        return "success";
    }

}
```

File `AddOfbizDemo.ftl`


```xml
<form method="post" action="<@ofbizUrl>createOfbizDemoEventFtl</@ofbizUrl>" name="createOfbizDemoEvent" class="form-horizontal">
  <div class="control-group">
    <label class="control-label" for="ofbizDemoTypeId">${uiLabelMap.OfbizDemoType}</label>
    <div class="controls">
      <select id="ofbizDemoTypeId" name="ofbizDemoTypeId">
        <#list ofbizDemoTypes as demoType>
          <option value='${demoType.ofbizDemoTypeId}'>${demoType.description}</option>
        </#list>
      </select>
    </div>
  </div>
  <div class="control-group">
    <label class="control-label" for="firstName">${uiLabelMap.OfbizDemoFirstName}</label>
    <div class="controls">
      <input type="text" id="firstName" name="firstName" required>
    </div>
  </div>
  <div class="control-group">
    <label class="control-label" for="lastName">${uiLabelMap.OfbizDemoLastName}</label>
    <div class="controls">
      <input type="text" id="lastName" name="lastName" required>
    </div>
  </div>
  <div class="control-group">
    <label class="control-label" for="comments">${uiLabelMap.OfbizDemoComment}</label>
    <div class="controls">
      <input type="text" id="comments" name="comments">
    </div>
  </div>
  <div class="control-group">
    <div class="controls">
      <button type="submit" class="btn">${uiLabelMap.CommonAdd}</button>
    </div>
  </div>
</form>
```


File `ListOfbizDemo.ftl`

```xml
<table class="table table-bordered table-striped table-hover">
    <thead>
    <tr>
        <th>${uiLabelMap.OfbizDemoId}</th>
        <th>${uiLabelMap.OfbizDemoType}</th>
        <th>${uiLabelMap.OfbizDemoFirstName}</th>
        <th>${uiLabelMap.OfbizDemoLastName}</th>
        <th>${uiLabelMap.OfbizDemoComment}</th>
    </tr>
    </thead>
    <tbody>
    <#list ofbizDemoList as ofbizDemo>
        <tr>
            <td>${ofbizDemo.ofbizDemoId}</td>
            <td>${ofbizDemo.getRelatedOne("OfbizDemoType").get("description", locale)}</td>
            <td>${ofbizDemo.firstName?default("NA")}</td>
            <td>${ofbizDemo.lastName?default("NA")}</td>
            <td>${ofbizDemo.comments!}</td>
        </tr>
    </#list>
    </tbody>
</table>
```

File `PostBody.ftl`

```xml
<#-- Close the tags opened in the PreBody section -->
</div>
</div>
</div>
</div>
<h4 align="center"> ==================Page PostBody and Page body in general ends here from Decorator Screen=========================</h4>

</body>
</html>
```

File `PreBody.ftl`

```xml
<html>
<head>
    <title>${layoutSettings.companyName}</title>
    <meta name="viewport" content="width=device-width, user-scalable=no"/>
    <#if webSiteFaviconContent?has_content>
        <link rel="shortcut icon" href="">
    </#if>
    <#list layoutSettings.styleSheets as styleSheet>
        <link rel="stylesheet" href="${StringUtil.wrapString(styleSheet)}" type="text/css"/>
    </#list>
    <#list layoutSettings.javaScripts as javaScript>

        <script type="text/javascript" src="${StringUtil.wrapString(javaScript)}"></script>
    </#list>
    <#--<script type="text/javascript" src="/ofbizDemo/js/jquery.min.js"></script>
    <script type="text/javascript" src="/ofbizDemo/js/bootstrap.min.js"></script>-->


</head>
<body data-offset="125">
<h4 align="center"> ==================Page PreBody Starts From Decorator Screen========================= </h4>
<div class="container menus" id="container">
    <div class="row">
        <div class="col-sm-12">
            <ul id="page-title" class="breadcrumb">
                <li>
                    <a href="<@ofbizUrl>main</@ofbizUrl>">Main</a>
                </li>
                <li class="active"><span class="flipper-title">${StringUtil.wrapString(uiLabelMap[titleProperty])}</span></li>
                <li class="pull-right">
                    <a href="<@ofbizUrl>logout</@ofbizUrl>" title="${uiLabelMap.CommonLogout}">logout</i></a>
                </li>
            </ul>
        </div>
    </div>
    <div class="row">
        <div class="col-lg-12 header-col">
            <div id="main-content">
                <h4 align="center"> ==================Page PreBody Ends From Decorator Screen=========================</h4>
                <h4 align="center"> ==================Page Body starts From Screen=========================</h4>
```

File `ListOfbizDemo.groovy`

```groovy
ofbizDemoTypes = delegator.findList("OfbizDemoType", null, null, null, null, false);
context.ofbizDemoTypes = ofbizDemoTypes;
ofbizDemoList = delegator.findList("OfbizDemo", null, null, null, null, false);
context.ofbizDemoList = ofbizDemoList;
```

File `controller.xml`

```xml
<?xml version="1.0" encoding="UTF-8"?>

<site-conf xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xmlns="http://ofbiz.apache.org/Site-Conf" xsi:schemaLocation="http://ofbiz.apache.org/Site-Conf http://ofbiz.apache.org/dtds/site-conf.xsd">
    <!-- The controller elements that are common to all OFBiz components
         can be found in the following xml file. A component can override the
         elements found in the common-controller.xml file. -->
    <include location="component://common/webcommon/WEB-INF/common-controller.xml"/>

    <description>OfbizDemo Component Site Configuration File</description>

    <!-- Events to run on every request before security (chains exempt) -->
    <!--
    <preprocessor>
    </preprocessor>
    -->
    <!-- Events to run on every request after all other processing (chains exempt) -->
    <!--
    <postprocessor>
        <event name="test" type="java" path="org.apache.ofbiz.webapp.event.TestEvent" invoke="test"/>
    </postprocessor>
    -->

    <!-- Request Mappings -->
    <request-map uri="main"><security https="true" auth="true"/><response name="success" type="view" value="main"/></request-map>
    
    <request-map uri="createOfbizDemo">
	    <security https="true" auth="true"/>
	    <event type="service" invoke="createOfbizDemo"/>
	    <response name="success" type="view" value="main"/>
    </request-map>

    <request-map uri="AddOfbizDemoFtl">
        <security https="true" auth="true"/>
        <response name="success" type="view" value="AddOfbizDemoFtl"/>
    </request-map>

    <request-map uri="FindOfbizDemo">
        <security https="true" auth="true"/>
        <response name="success" type="view" value="FindOfbizDemo"/>
     </request-map>

    <request-map uri="createOfbizDemoEvent">
        <security https="true" auth="true"/>
        <event type="java" path="com.companyname.ofbizdemo.events.OfbizDemoEvents" invoke="createOfbizDemoEvent"/>
        <response name="success" type="view" value="main"/>
        <response name="error" type="view" value="main"/>
    </request-map>

    <request-map uri="createOfbizDemoEvent2">
        <security https="true" auth="true"/>
        <event type="service" invoke="createOfbizDemoByGroovyService"/>
        <response name="success" type="view" value="AddOfbizDemoFtl"/>
        <response name="error" type="view" value="AddOfbizDemoFtl"/>
    </request-map>
   
	<!-- View Mapping -->
	<view-map name="FindOfbizDemo" type="screen" page="component://ofbizDemo/widget/OfbizDemoScreens.xml#FindOfbizDemo"/>
    

	 
	<!--View Mapping-->
	<view-map name="AddOfbizDemoFtl" type="screen" page="component://ofbizDemo/widget/OfbizDemoScreens.xml#AddOfbizDemoFtl"/>
       
    
    <!-- View Mappings -->
    <view-map name="main" type="screen" page="component://ofbizDemo/widget/OfbizDemoScreens.xml#main"/>
</site-conf>
```

File `web.xml`

```xml
<?xml version="1.0" encoding="UTF-8"?>

<web-app version="4.0" xmlns="http://xmlns.jcp.org/xml/ns/javaee" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd">
    <display-name>Apache OFBiz - OfbizDemo Component</display-name>
    <description>OfbizDemo Component of the Apache OFBiz Project</description>

    <!-- context-param>
        <param-name>webSiteId</param-name>
        <param-value>ofbizDemoSite</param-value>
        <description>A unique ID used to look up the WebSite entity. Only for component using a WebSite entity, like ecommerce</description>
    </context-param-->
    <context-param>
        <description>A unique name used to identify/recognize the local dispatcher for the Service Engine</description>
        <param-name>localDispatcherName</param-name><param-value>ofbizDemo</param-value>
    </context-param>
    <context-param>
        <description>The Name of the Entity Delegator to use, defined in entityengine.xml</description>
        <param-name>entityDelegatorName</param-name><param-value>default</param-value>
    </context-param>
    <context-param>
        <description>The location of the main-decorator screen to use for this webapp; referred to as a context variable in screen def XML files.</description>
        <param-name>mainDecoratorLocation</param-name>
        <param-value>component://ofbizDemo/widget/CommonScreens.xml</param-value>
    </context-param>
    <context-param>
        <description>Remove unnecessary whitespace from HTML output.</description>
        <param-name>compressHTML</param-name>
        <param-value>false</param-value>
    </context-param>

    <filter>
        <display-name>ControlFilter</display-name>
        <filter-name>ControlFilter</filter-name>
        <filter-class>org.apache.ofbiz.webapp.control.ControlFilter</filter-class>
        <init-param>
            <param-name>allowedPaths</param-name>
            <param-value>/error:/control:/select:/index.html:/index.jsp:/default.html:/default.jsp:/images:/includes/maincss.css:/css:/js</param-value>
        </init-param>
        <init-param><param-name>redirectPath</param-name><param-value>/control/main</param-value></init-param>
    </filter>
    <filter>
        <display-name>ContextFilter</display-name>
        <filter-name>ContextFilter</filter-name>
        <filter-class>org.apache.ofbiz.webapp.control.ContextFilter</filter-class>
    </filter>
    <filter>
        <display-name>SameSiteFilter</display-name>
        <filter-name>SameSiteFilter</filter-name>
        <filter-class>org.apache.ofbiz.webapp.control.SameSiteFilter</filter-class>
    </filter>    
    <filter-mapping><filter-name>ControlFilter</filter-name><url-pattern>/*</url-pattern></filter-mapping>
    <filter-mapping><filter-name>ContextFilter</filter-name><url-pattern>/*</url-pattern></filter-mapping>
    <filter-mapping><filter-name>SameSiteFilter</filter-name><url-pattern>/*</url-pattern></filter-mapping>

    <listener><listener-class>org.apache.ofbiz.webapp.control.ControlEventListener</listener-class></listener>
    <listener><listener-class>org.apache.ofbiz.webapp.control.LoginEventListener</listener-class></listener>
    <!-- NOTE: not all app servers support mounting implementations of the HttpSessionActivationListener interface -->
    <!-- <listener><listener-class>org.apache.ofbiz.webapp.control.ControlActivationEventListener</listener-class></listener> -->

    <servlet>
        <description>Main Control Servlet</description>
        <display-name>ControlServlet</display-name>
        <servlet-name>ControlServlet</servlet-name>
        <servlet-class>org.apache.ofbiz.webapp.control.ControlServlet</servlet-class>
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping><servlet-name>ControlServlet</servlet-name><url-pattern>/control/*</url-pattern></servlet-mapping>

    <session-config>
        <session-timeout>60</session-timeout><!-- in minutes -->
    </session-config>

    <welcome-file-list>
        <welcome-file>index.jsp</welcome-file>
        <welcome-file>index.html</welcome-file>
        <welcome-file>index.htm</welcome-file>
    </welcome-file-list>
</web-app>
```

File `index.jsp`

```html
<%response.sendRedirect("control/main");%>
```

File `CommonScreens.xml`

```xml
<?xml version="1.0" encoding="UTF-8"?>

<screens xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xmlns="http://ofbiz.apache.org/Widget-Screen" xsi:schemaLocation="http://ofbiz.apache.org/Widget-Screen http://ofbiz.apache.org/dtds/widget-screen.xsd">

    <screen name="main-decorator">
        <section>
            <actions>
                <property-map resource="OfbizDemoUiLabels" map-name="uiLabelMap" global="true"/>
                <property-map resource="CommonUiLabels" map-name="uiLabelMap" global="true"/>

                <set field="layoutSettings.companyName" from-field="uiLabelMap.OfbizDemoCompanyName" global="true"/>
                <set field="layoutSettings.companySubtitle" from-field="uiLabelMap.OfbizDemoCompanySubtitle" global="true"/>

                <set field="activeApp" value="ofbizDemo" global="true"/>
                <set field="applicationMenuName" value="MainAppBar" global="true"/>
                <set field="applicationMenuLocation" value="component://ofbizDemo/widget/OfbizDemoMenus.xml" global="true"/>
                <set field="applicationTitle" from-field="uiLabelMap.OfbizDemoApplication" global="true"/>
            </actions>
            <widgets>
                <include-screen name="GlobalDecorator" location="component://common/widget/CommonScreens.xml"/>
            </widgets>
        </section>
    </screen>

    <screen name="OfbizDemoCommonDecorator">
        <section>
            <actions>
            </actions>
            <widgets>
                <decorator-screen name="main-decorator" location="${parameters.mainDecoratorLocation}">
                    <decorator-section name="body">
                        <section>
                            <condition>
                                <if-has-permission permission="OFBIZDEMO" action="_VIEW"/>
                            </condition>
                            <widgets>
                                <decorator-section-include name="body"/>
                            </widgets>
                            <fail-widgets>
                                <label style="h3">${uiLabelMap.OfbizDemoViewPermissionError}</label>
                            </fail-widgets>
                        </section>
                    </decorator-section>
                </decorator-screen>
            </widgets>
        </section>
    </screen>

    <screen name="OfbizDemoCommonDecorator">
        <section>
            <actions>
                <property-map resource="OfbizDemoUiLabels" map-name="uiLabelMap" global="true"/>
                <property-map resource="CommonUiLabels" map-name="uiLabelMap" global="true"/>

                <set field="layoutSettings.companyName" from-field="uiLabelMap.OfbizDemoCompanyName" global="true"/>

                <!-- Including custom CSS Styles that you want to use in your application view. [] in field can be used to
                     set the order of loading CSS files to load if there are multiple -->
                <set field="layoutSettings.styleSheets[]" value="/ofbizDemo/css/bootstrap.min.css"/>

                <!-- Including custom JS that you want to use in your application view. [] in field can be used to
                     set the order of loading of JS files to load if there are multiple -->
                <set field="layoutSettings.javaScripts[+0]" value="/ofbizDemo/js/jquery.min.js" global="true"/>
                <set field="layoutSettings.javaScripts[+1]" value="/ofbizDemo/js/bootstrap.min.js" global="true"/>
                <set field="titleProperty" value="PageTitleFindOfbizDemo"/>
            </actions>
            <widgets>
                <section>
                    <condition>
                        <if-has-permission permission="OFBIZDEMO" action="_VIEW"/>
                    </condition>
                    <widgets>
                        <platform-specific><html><html-template location="component://ofbizDemo/webapp/ofbizDemo/includes/PreBody.ftl"/></html></platform-specific>
                        <decorator-section-include name="pre-body"/>
                        <decorator-section-include name="body"/>
                        <platform-specific><html><html-template location="component://ofbizDemo/webapp/ofbizDemo/includes/PostBody.ftl"/></html></platform-specific>
                    </widgets>
                    <fail-widgets>
                        <label style="h3">${uiLabelMap.OfbizDemoViewPermissionError}</label>
                    </fail-widgets>
                </section>
            </widgets>
        </section>
    </screen>
</screens>
```

File `OfbizDemoForms.xml`

```xml
<?xml version="1.0" encoding="UTF-8"?>

<forms xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns="http://ofbiz.apache.org/Widget-Form"
	xsi:schemaLocation="http://ofbiz.apache.org/Widget-Form http://ofbiz.apache.org/dtds/widget-form.xsd">

	<form name="AddOfbizDemo" type="single" target="createOfbizDemo">	
		<auto-fields-service service-name="createOfbizDemo" />

		<field name="ofbizDemoTypeId" title="${uiLabelMap.CommonType}">
			<drop-down allow-empty="false" current-description="">
				<entity-options description="${description}" key-field-name="ofbizDemoTypeId" entity-name="OfbizDemoType">
					<entity-order-by field-name="description" />
				</entity-options>
			</drop-down>
		</field>

		<field name="submitButton" title="${uiLabelMap.CommonAdd}">
			<submit button-type="button" />
		</field>
	</form>
	
	<form name="FindOfbizDemo" type="single" target="FindOfbizDemo" default-entity-name="OfbizDemo">
    <field name="noConditionFind"><hidden value="Y"/> <!-- if this isn't there then with all fields empty no query will be done --></field>
    <field name="ofbizDemoId" title="${uiLabelMap.OfbizDemoId}"><text-find/></field>
    <field name="firstName" title="${uiLabelMap.OfbizDemoFirstName}"><text-find/></field>
    <field name="lastName" title="${uiLabelMap.OfbizDemoLastName}"><text-find/></field>
    <field name="ofbizDemoTypeId" title="${uiLabelMap.OfbizDemoType}">
        <drop-down allow-empty="true" current-description="">
            <entity-options description="${description}" key-field-name="ofbizDemoTypeId" entity-name="OfbizDemoType">
                <entity-order-by field-name="description"/>
            </entity-options>
        </drop-down>
    </field>
    <field name="searchButton" title="${uiLabelMap.CommonFind}" widget-style="smallSubmit"><submit button-type="button" image-location="/images/icons/magnifier.png"/></field>
</form>
  
    <form name="ListOfbizDemo" type="list" list-name="listIt" paginate-target="FindOfbizDemo" default-entity-name="OfbizDemo" separate-columns="true"
    odd-row-style="alternate-row" header-row-style="header-row-2" default-table-style="basic-table hover-bar">
	    <actions>
	       <!-- Preparing search results for user query by using OFBiz stock service to perform find operations on a single entity or view entity -->
	       <service service-name="performFind" result-map="result" result-map-list="listIt">
	           <field-map field-name="inputFields" from-field="ofbizDemoCtx"/>
	           <field-map field-name="entityName" value="OfbizDemo"/>
	           <field-map field-name="orderBy" from-field="parameters.sortField"/>
	           <field-map field-name="viewIndex" from-field="viewIndex"/>
	           <field-map field-name="viewSize" from-field="viewSize"/>
	        </service>
	    </actions>
	    <field name="ofbizDemoId" title="${uiLabelMap.OfbizDemoId}"><display/></field>
	    <field name="ofbizDemoTypeId" title="${uiLabelMap.OfbizDemoType}"><display-entity entity-name="OfbizDemoType"/></field>
	    <field name="firstName" title="${uiLabelMap.OfbizDemoFirstName}" sort-field="true"><display/></field>
	    <field name="lastName" title="${uiLabelMap.OfbizDemoLastName}" sort-field="true"><display/></field>
	    <field name="comments" title="${uiLabelMap.OfbizDemoComment}"><display/></field>
    </form>
	
</forms>
```

File `OfbizDemoMenus.xml`

```xml
<?xml version="1.0" encoding="UTF-8"?>

<menus xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://ofbiz.apache.org/Widget-Menu" xsi:schemaLocation="http://ofbiz.apache.org/Widget-Menu http://ofbiz.apache.org/dtds/widget-menu.xsd">
    <menu name="MainAppBar" title="${uiLabelMap.OfbizDemoApplication}" extends="CommonAppBarMenu" extends-resource="component://common/widget/CommonMenus.xml">
        <menu-item name="main" title="${uiLabelMap.CommonMain}"><link target="main"/></menu-item>
        <menu-item name="findOfbizDemo" title="${uiLabelMap.OfbizDemoFind}"><link target="FindOfbizDemo"/></menu-item>
        
        <menu-item name="addOfbizDemoFtl" title="${uiLabelMap.OfbizDemoAddFtl}"><link target="AddOfbizDemoFtl"/></menu-item> 
        
        
    </menu>
</menus>
```

File `OfbizDemoScreens.xml`

```xml
<?xml version="1.0" encoding="UTF-8"?>

<screens xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xmlns="http://ofbiz.apache.org/Widget-Screen" xsi:schemaLocation="http://ofbiz.apache.org/Widget-Screen http://ofbiz.apache.org/dtds/widget-screen.xsd">

    <screen name="main">
        <section>
            <actions>
                <set field="headerItem" value="main"/><!-- this highlights the selected menu-item with name "main" -->
            </actions>
            <widgets>
                <decorator-screen name="OfbizDemoCommonDecorator" location="${parameters.mainDecoratorLocation}">
                    <decorator-section name="body">
                    
                        <screenlet title="Add Ofbiz Demo">
                            <include-form name="AddOfbizDemo" location="component://ofbizDemo/widget/OfbizDemoForms.xml"/>
                        </screenlet>
                    
                    </decorator-section>
                </decorator-screen>
            </widgets>
        </section>
    </screen>

	<!-- Find and list all ofbizdemos in a tabular format -->
	<screen name="FindOfbizDemo">
	    <section>
	        <actions>
	            <set field="headerItem" value="findOfbizDemo"/>
	            <set field="titleProperty" value="PageTitleFindOfbizDemo"/>
	            <set field="ofbizDemoCtx" from-field="parameters"/>
	        </actions>
	        <widgets>
	            <decorator-screen name="main-decorator" location="${parameters.mainDecoratorLocation}">
	                <decorator-section name="body">
	                    <section>
	                        <condition>
	                            <if-has-permission permission="OFBIZDEMO" action="_VIEW"/>
	                        </condition>
	                        <widgets>
	                            <decorator-screen name="FindScreenDecorator" location="component://common/widget/CommonScreens.xml">
	                                <decorator-section name="search-options">
	                                    <include-form name="FindOfbizDemo" location="component://ofbizDemo/widget/OfbizDemoForms.xml"/>
	                                </decorator-section>
	                                <decorator-section name="search-results">
	                                    <include-form name="ListOfbizDemo" location="component://ofbizDemo/widget/OfbizDemoForms.xml"/>
	                                </decorator-section>
	                            </decorator-screen>
	                        </widgets>
	                        <fail-widgets>
	                            <label style="h3">${uiLabelMap.OfbizDemoViewPermissionError}</label>
	                       </fail-widgets>
	                    </section>
	                </decorator-section>
	            </decorator-screen>
	        </widgets>
	    </section>
	</screen>

	<screen name="AddOfbizDemoFtl">
		<section>
			<actions>
				<set field="titleProperty" value="OfbizDemoAddOfbizDemoFtl"/>
				<set field="headerItem" value="addOfbizDemoFtl"/>
				<script location="component://ofbizDemo/webapp/ofbizDemo/WEB-INF/actions/crud/ListOfbizDemo.groovy"/>
			</actions>
			<widgets>
				<decorator-screen name="OfbizDemoCommonDecorator" location="${parameters.mainDecoratorLocation}">
					<decorator-section name="body">
						<label style="h4" text="${uiLabelMap.OfbizDemoListOfbizDemos}"/>
						<platform-specific>
							<html><html-template location="component://ofbizDemo/webapp/ofbizDemo/crud/ListOfbizDemo.ftl"/></html>
						</platform-specific>
						<label style="h4" text="${uiLabelMap.OfbizDemoAddOfbizDemoFtl}"/>
						<platform-specific>
							<html><html-template location="component://ofbizDemo/webapp/ofbizDemo/crud/AddOfbizDemo.ftl"/></html>
						</platform-specific>
					</decorator-section>
				</decorator-screen>
			</widgets>
		</section>
	</screen>
</screens>
```

File `ofbiz-component.xml`

```xml
<?xml version="1.0" encoding="UTF-8"?>

<ofbiz-component name="ofbizDemo"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:noNamespaceSchemaLocation="http://ofbiz.apache.org/dtds/ofbiz-component.xsd">
    <!-- define resource loaders; most common is to use the component resource loader -->
    <resource-loader name="main" type="component"/>

    <!-- place the config directory on the classpath to access configuration files -->
    <classpath type="dir" location="config"/>
    <!-- entity resources: model(s), eca(s), group, and data definitions -->
    <entity-resource type="model" reader-name="main" loader="main" location="entitydef/entitymodel.xml"/>
    <!-- <entity-resource type="eca" reader-name="main" loader="main" location="entitydef/eecas.xml"/> -->
    <entity-resource type="data" reader-name="seed" loader="main" location="data/OfbizDemoTypeData.xml"/>
    <entity-resource type="data" reader-name="seed" loader="main" location="data/OfbizDemoSecurityPermissionSeedData.xml"/>
    <entity-resource type="data" reader-name="demo" loader="main" location="data/OfbizDemoSecurityGroupDemoData.xml"/>
    <entity-resource type="data" reader-name="demo" loader="main" location="data/OfbizDemoDemoData.xml"/>

    <!-- service resources: model(s), eca(s) and group definitions -->
    <service-resource type="model" loader="main" location="servicedef/services.xml"/>
    <!--
    <service-resource type="eca" loader="main" location="servicedef/secas.xml"/>
    <service-resource type="group" loader="main" location="servicedef/groups.xml"/>
    -->

    <test-suite loader="main" location="testdef/OfbizDemoTests.xml"/>

    <!-- web applications; will be mounted when using the embedded container -->
    <webapp name="ofbizDemo"
        title="OfbizDemo"
        server="default-server"
        location="webapp/ofbizDemo"
        base-permission="OFBTOOLS,OFBIZDEMO"
        mount-point="/ofbizDemo"/>
</ofbiz-component>
```