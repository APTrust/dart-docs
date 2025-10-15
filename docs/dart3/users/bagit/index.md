# BagIt Profiles

While the [BagIt specification](https://tools.ietf.org/html/rfc8493) describes the general requirements for a valid bag, BagIt profiles describe the tags, manifests, and tag manifests required to make a valid bag for a specific organization or purpose.

DART uses BagIt profiles to produce bags that adhere to a profile, and to validate that bags do adhere to the profile.

<iframe width="845" height="518" src="https://www.youtube.com/embed/qnFFlRhlfCA" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## DART BagIt Profiles

While the general specification for BagIt Profiles can be found on [GitHub](https://github.com/bagit-profiles/bagit-profiles), DART's BagIt profiles differ from the published specification in the following ways:

1. DART profiles use camel case identifiers with no hyphens or periods in attribute names. For example, the `Allow-Fetch.txt` in GitHub BagIt profiles is called `allowFetchTxt` in DART profiles. This is in part to simplify JavaScript attributes so they can be reference in dot notation, and in part because the nedb object database used in early versions of DART did not support attribute names containing dots.

1. DART profiles include an `id` attribute with a UUID. This is used internally, while the bagItProfileIdentifier URL is used externally.

1. DART profiles include the following additional boolean attributes:
       1. `allowMiscTopLevelFiles` which indicates whether files other than
           manifests and tag manifests are allowed in the bag's root directory.
       1. `allowMiscDirectories` which indicates whether directories other than
           /data and its children are allowed in the bag.
       1. `tarDirMustMatchName` which indicates whether the name of the
           unserialized bag must match the name of the serialized bag, minus
           the serialization extension. (That is, whether my_bag.tar must untar
           to my_bag, and my_bag.zip must unzip to my_bag.)

1. DART includes the attribute `baseProfileId` for internal use, to know whether
   a user-created profile was based on an existing profile.

1. DART includes the `isBuiltIn` attribute to indicate that a profile was built
   in to the application (usually through a setup module or migration). These
   profiles cannot be deleted.

1. DART does not specify `tagFilesRequired` or `tagFilesAllowed`.

1. DART BagIt profiles specify all tag requirements in a single list called
   `tags` while the GitHub BagIt spec defines them as nested objects with
   arbitrary names. The single list of uniform objects in the DART model makes
   tag definitions easier to manipulate.

1. While tag definitions in the GitHub BagIt Profile spec include only the
   attributes `required` and `values`, DART tag definitions include the following
   attributes:
       1. `id` - A unique identifier in UUID format that DART uses internally.
           This allows users to edit tag definitions in the DART UI without
           the system losing track of which tag is being edited. The UUID is
           immutable while all other attributes are not.
       1. `tagFile` - The name of the tag file that contains the tag. This is
           a path relative to the bag root. For example, `bag-info.txt` or
           `custom-tags/image-credits.txt`.
       1. `tagName` - The name of the tag. For example, `Source-Organization`.
       1. `required` - A boolean indicating whether the tag is required.
       1. `values` - An option list of legal values. If this list is present
           and a tag contains a value that's not in the list, the value and
           the bag are invalid.
       1. `defaultValue` - A default value assigned by the user to the tag.
           DART's BagIt Profile editor allows users to specify default values
           to tags that will be consistent across bags. For example, users
           can define a default value to `Source-Organization` so they don't
           have to assign it repeatedly every time they create a new bag.
       1. `userValue` - The value of a tag to be written into or read from a
           tag file. Users can specify a userValue that overrides the
           defaultValue when they create a bag. When reading a bag, DART
           assigns the actual value of a tag to what was read from the bag.
       1. `isBuiltIn` - A boolean value indicating whether a tag definition
           is built in (as opposed to user-created). DART's BagIt Profile
           editor allows users to add custom tags to a published profile,
           such as the APTrust profile, while preventing them from deleting
           built-in tag definitions. Deleting a built-in tag definition such
           as `Source-Organization` would lead to DART generating invalid bags.
       1. `help` - Help text to describe the significance of the tag to the
           user. If present, the DART UI will display this message for the
           user's edification and delight.


## Built-in Profiles

DART includes the following built-in profiles. Note that the BagIt Profile editor allows you to clone and customize each of these, though customization is limited to adding tags and tag files, and setting default tag values.

* APTrust - The standard APTrust BagIt profile.

* BTR - The Beyond the Repository BagIt profile, which will be accepted by a number of distributed digital preservation repositories.

* Empty Profile - A BagIt profile that defines the basic tags of the BagIt specification but does not require any of them. You can clone use this profile to create valid generic bags.

## Custom Profiles

DART enables users to create new BagIt profiles from scratch, and to clone and modify existing profiles.

See also: [Creating BagIt Profiles](creating.md), [Customizing BagIt Profiles](customizing.md)
