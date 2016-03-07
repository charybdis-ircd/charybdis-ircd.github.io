---
layout: post
title:  "The new module API and what it means for you"
---

Since ratbox, `DECLARE_MODULE_AV1` has been the way that the symbol for the module header has been created. It has stayed stable throughout 10 years of its existence in Charybdis and longer in Ratbox.

Although Charybdis itself has never really had a stable ABI, the **API** has largely had additive changes, and maybe the occasional parameter addition to a function or addition to a struct. Thus, it has remained effectively compatible for many years.

This update drastically breaks with the past and is a big leap forward for Charybdis. Extension authors and those who work on derivatives of Charybdis should take note!

Enter AV2
=========

"AV2" is what we have dubbed the new module header API, since you use `DECLARE_MODULE_AV2` to use it. It adds and alters the following fields in the header declaration:

* `mapi_module_version` can now be NULL with no ill consequences - it is replaced with the current IRC daemon version if NULL. Leaving it NULL is only encouraged for bundled extensions. The rationale for this change is that the "version" tag was meant for the bad old days of CVS and SVN. As a result, most of the tags had not been updated for over 7 years (or even since Ratbox).
* `mapi_cap_list` is now an entry in the header of type `mapi_cap_list_av2`. This was to avoid the boilerplate that was cropping up all over the place resulting in needless module initialisation functions with this sole purpose. More on this below.
* `mapi_module_description` is now an entry in the header of type `const char *`. It provides a human-readable description of the module for administrators.
* The provenance of a module, called its origin, is tracked by the ircd based on how it is loaded. This is shown in /MODLIST and will be used soon to open up a partial version of /MODLIST to **all** users.

What this means for administrators
==================================

This means that administrators will see human-readable descriptions for all the modules in Charybdis when they use /MODLIST, and any third-party modules using AV2 that provide a description. This also means that a partial view of /MODLIST will soon be open to all users for transparency purposes. Users will not be able to see module addresses (as this presents a possible attack vector for exploits), but will be able to see what modules are loaded and what they do.

We believe this is a positive thing as it will keep IRC operators who use Charybdis honest about what they're really running on their networks. There is nothing to be gained by obfuscating this watered-down MODLOAD, other than hiding abusive features from users (which we consider unethical).

What this means for users
=========================

As aforementioned, users will soon be able to use /MODLIST to see potentially abusive modules. This will help users make informed decisions about their network choices in an era where privacy is often at a premium. Remember that encryption is only as secure as the server you're using.

What this means for module developers
=====================================

For the time being, AV1 will remain supported. However, it is encouraged to move to AV2 eventually for the benefits it brings, including description strings and CAP lists. AV1 should be considered deprecated and slated for removal in 4.0, as nothing in Charybdis relies on it now. Old modules will continue to work for the time being.

CAP lists
---------

This feature allows module authors to make lists of capabilities, client or server, their module will support, like so:

```C
unsigned int CAP_TESTCAP_SERVER, CAP_TESTCAP_CLIENT;
mapi_cap_list_av2 test_cap_list[] = {
  { MAPI_CAP_SERVER, "TESTCAP", NULL, &CAP_TESTCAP_SERVER },
  { MAPI_CAP_CLIENT, "testcap", NULL, &CAP_TESTCAP_CLIENT },
  { 0, NULL, NULL, NULL }
};
```

Planned features
----------------

We aren't done yet with AV2. We have some more exciting features planned:

* Easier way of defining channel modes similar to how CAPs are defined, and more flexibility in mode types
* Possibly cleaner `DECLARE_MODULE_AV2` declaration
* Charybdis-specifc versioning embedded into each module upon build, so the loader can detect incompatible modules.

I also have some other ideas of my own, but those may come at a later date.

If you want to wait for AV2 to stabilise before updating, that is perfectly fine. We don't anticipate much more churn, though.
