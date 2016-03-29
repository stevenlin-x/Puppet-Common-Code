Puppet Common Code
==============

List of some commonly used Puppet code blocks.

### Files and Directory

#### Ensure a directory is present with specified properties, if not, create the directory. Also create all intermediate directories if applicable.

````text
file { 'directory-name':
	path => '/path/to/directory',
	ensure => 'directory',
	mode  => '0644',
	recurse => true,
}
````

#### Ensure a directory is present, if not, create the directory and copy the contents from the source directory

````text
file { 'directory-name':
	path => '/path/to/directory',
	ensure => 'directory',
	mode  => '0644',
	recurse => true,
	source => "${source-directory-path}/file-name",
}
````

#### Ensure a file is present
````text
file { '/path/to/file':
	ensure => 'file',
}
````

#### Ensure the file is present and copy its contents from the source directory
````text
file { '/path/to/file':
	ensure => 'file',
	source => "${source-directory-path}/file",
}
````

#### Create a symlink file
````text
file { '/path/to/symlink/file':
	ensure => 'link',
	target => '/path/to/target/of/symlink/file',
}
````

#### Create a symlink file but make sure the directory/file block specified is valid (e.g. the directory specified is present) before doing so
````text
file { '/path/to/symlink/file':
	ensure => 'link',
	target => '/path/to/target/of/symlink/file',
	require	=> File['directory-name'],
}
````

### Users and Groups

#### Ensure a group is present
````text
group { 'samplegroup':
	ensure => "present",
}
````

#### Ensure a user is present with specified properties and is assigned to the specified group
````text
user { 'sampleuser':
	ensure => 'present',
	home => '/home/sampleuser',
	shell => '/bin/bash',
	gid => 'sampleuser',
	require => Group['samplegroup'],
}
````

### Services

#### Start the service, enable the service on boot time, and refreshes (i.e. restart) the service if the File block subscribed to changes.

##### Note: The var $service_name should contain the pre-determined service name string specific to the operating system.

````text
service { 'service-name':
	name	=> $service_name,
	ensure	=> running,
	enable	=> true,
	require	=> [ Package['Pack1'], Package['Pack2'] ],
	subscribe => File['file.conf'],
}
````

#### Turn off the service and disable the service at boot time
````text
service { 'service-name':
	ensure => stopped,
	enable => false,
}
````

### Packages

#### Ensure the service is installed from the specified package repository (e.g. Yum used in this example).
````text
package { 'service-name':
	provider => 'yum',
	ensure => installed,
}
````