Authenticate user on main wiki based on Jasig CAS server. It creates XWiki users if they have never logged in before and synchronizes membership to XWiki groups based on membership to CAS group field mapping. It supports CAS 2.0 and CAS 3.0 protocol. CAS 3.0 protocol can be used for attributes and group membership synchronization.

# Configuration (xwiki.cfg)

	# CAS authentication
	xwiki.authentication.authclass=org.xwiki.contrib.authentication.cas.XWikiCASAuthenticator

	# CAS server url (i.e. https://localhost:8443/cas)
	xwiki.authentication.cas.server=https://localhost:8443/cas

	# possible values are CAS20 or CAS30
	xwiki.authentication.cas.protocol=CAS30

	# user not authorized page (i.e. /bin/view/XWiki/XWikiCASAccessDenied). If not set a HTTP status 401 is returned.
	#xwiki.authentication.cas.access_denied_page=/bin/view/XWiki/XWikiCASAuthFailed

	# (only CAS30) mapping between XWiki user profile values and CAS attributes. Example (xwiki-attribute=cas-attribute,...)
	xwiki.authentication.cas.fields_mapping=last_name=lastName,first_name=firstName,email=email

	# 0 or 1 if create XWiki user after log in
	xwiki.authentication.cas.create_user=1

	# 0 or 1 if update user attributes after every log in
	xwiki.authentication.cas.update_user=1

	# (only CAS30) CAS attribute name which contains group membership
	xwiki.authentication.cas.group_field=roles

	# (only CAS30) Maps XWiki groups to CAS groups, separator is "|".
	xwiki.authentication.cas.group_mapping=XWiki.XWikiAdminGroup=cn=AdminRole,ou=groups,o=domain,c=com|\
                                         XWiki.CASUsers=ou=groups,o=domain,c=com|\
                                         XWiki.Organisation=cn=Org1,ou=groups,o=domain,c=com

# Install

* In a terminal, change to the project's directory, and run:
```bash
mvn clean package
```
* Setup xwiki.cfg

# TODO

* Logout from CAS is not implemented yet. As workaround you could change menuview.vm in your skin. Change the generation of logout url from
	
	\#set ($logouturl = $xwiki.getURL('XWiki.XWikiLogout', 'logout', "xredirect=$escapetool.url($xwiki.relativeRequestURL)"))
	
	to something like
	
	\#set ($logouturl = $xwiki.getURL('XWiki.XWikiLogout', 'logout', "xredirect=$escapetool.url('https://localhost:8443/cas/logout')"))

