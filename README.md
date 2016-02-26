# Diagnosis #

Bug 510516 reported that the Application chooser on mailto: links is unclear. Ian Stirling, the original reporter of the bug was slightly vague in regards to what the actual issue was. However, upon observing the screenshot Ian provided, along with additional follow up comments, I have come to the conclusion that the bug is an UX problem.

As of Bug 510516, if a default Mail application is not selected and a mailto: link is clicked, a popup dialog will appear asking the user to select a Mail application to use. The intention of this dialog is to ask the user to either select an application from the list, or to browse the computer to find a different application by clicking the "Choose..." button. Once an application is selected, the "OK" button confirms the selection. However, this dialog may be confusing to some users. As Ian pointed out, the "Choose..." button may instead be misinterpreted as confirmation of the user's selection, rather than the "OK" button.

Fixing Bug 510516 will make the process of selecting a default Application more comprehensive to a wider range of users, thereby saving people from confusion and potential loss of productivity. 

A possible risk that may result from fixing Bug 510516 is that the fix could result in the opposite effect, and further increase confusion. Thus, it is important for the UX team to review the proposed fix.

# Solution #

According to official comments from the UX team themselves, the desired changes are:

* Change *Choose an Application* to *Choose other Application*
* Change *OK* to *Open in selected Application* (if that's too long we can also use *Open in Application* or just *Open link*)

Therefore, I will simply follow the official recommendations. I will locate the files responsible for the localization of the Application chooser, and make the necessary localization changes as outlined above.

# Testing #

In order to validate that the bug was fixed successfully, I will first make sure that Firefox currently does not have a default Mail application set. Then, I will either create or find a webpage that includes a mailto: link. Finally, I will click on the link and observer whether or not the the localization changes went through successfully.

Please observe the following screenshots to see the effects of my changes.

## Before ##

![Before](/before.jpg?raw=true)

## After ##

![Before](/after.jpg?raw=true)

# Patch #

	# HG changeset patch
	# User Wayne Zhang <wayne.zhang@mail.utoronto.ca>

	Changed text of application picker.


	diff --git a/toolkit/locales/en-US/chrome/mozapps/handling/handling.dtd b/toolkit/locales/en-US/chrome/mozapps/handling/handling.dtd
	index 71afc28..fe7a525 100644
	--- a/toolkit/locales/en-US/chrome/mozapps/handling/handling.dtd
	+++ b/toolkit/locales/en-US/chrome/mozapps/handling/handling.dtd
	@@ -4,6 +4,7 @@
	 
	 <!ENTITY window.emWidth "26em">
	 <!ENTITY window.emHeight "26em">
	-<!ENTITY ChooseApp.description "Choose an Application">
	+<!ENTITY ChooseOtherApp.description "Choose other Application">
	 <!ENTITY ChooseApp.label "Choose…">
	 <!ENTITY ChooseApp.accessKey "C">
	+<!ENTITY accept "Open link">
	diff --git a/toolkit/mozapps/handling/content/dialog.xul b/toolkit/mozapps/handling/content/dialog.xul
	index 89da1be..4a1bbb8 100644
	--- a/toolkit/mozapps/handling/content/dialog.xul
	+++ b/toolkit/mozapps/handling/content/dialog.xul
	@@ -34,7 +34,7 @@
	                  ondblclick="dialog.onDblClick();"
	                  onselect="dialog.updateOKButton();">
	       <richlistitem id="item-choose" orient="horizontal" selected="true">
	-        <label value="&ChooseApp.description;" flex="1"/>
	+        <label value="&ChooseOtherApp.description;" flex="1"/>
	         <button oncommand="dialog.chooseApplication();"
	                 label="&ChooseApp.label;" accesskey="&ChooseApp.accessKey;"/>
	       </richlistitem>
	@@ -44,4 +44,9 @@
	   <checkbox id="remember" aria-describedby="remember-text" oncommand="dialog.onCheck();"/>
	   <description id="remember-text"/>
	 
	+  <hbox class="dialog-button-box" align="right">
	+    <button dlgtype="cancel" icon="cancel" class="dialog-button"/>
	+    <button dlgtype="accept" label="&accept;" icon="open" class="dialog-button"/>
	+  </hbox>
	+
	 </dialog>

Alternatively see [Changed-text-of-application-picker.patch](/Changed-text-of-application-picker.patch?raw=true).
