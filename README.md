# Scribd Client
This is a Scribd client that's compatible with Composer. It can be found on Packagist: https://packagist.org/packages/nikkobautista/scribd
This was based on the original version created by Robert Pottorff.

# Installation
Just add the package to the ````require```` atttribute of your ````composer.json```` file like so:

````json
{
    "require": {
        "nikkobautista/scribd": "dev-master"
    },
}
````

# Usage

## Initialization
````php
$scribd_api_key = "ENTER-YOUR-API-KEY-HERE";
$scribd_secret = "ENTER-YOUR-API-SECRET-HERE"; 
$scribd = new Scribd\API($scribd_api_key, $scribd_secret);
````

## Uploading a document from a file
````php
$file = '../testfile.txt'; //a reference to the file in reference to the current working directory.
$doc_type = null;
$access = null;
$rev_id = null;
$data = $scribd->upload($file, $doc_type, $access, $rev_id); // returns Array ( [doc_id] => 1026598 [access_key] => key-23nvikunhtextwmdjm2i )
````

## Upload a document from a URL
````php
$scribd->my_user_id = '143'; # The user ID of one of your users
$url = 'http://lib.store.yahoo.net/lib/paulgraham/onlisp.ps';
$doc_type = null;
$access = "public";
$rev_id = null; // By using a id stored in a database, you can update an existing document without creating a new one
$data = $scribd->uploadFromUrl($url, $doc_type, $access, $rev_id); // returns Array ( [doc_id] => 1021237 [access_key] => key-dogbmich9x5iu09kiki )
````

## Get the list of documents
````php
$data = $scribd->getList();
/*
will return 
Array
(
    [result] => Array
    (
        [doc_id] => 1018558
        [access_key] => key-1s79um0eg9a355arn5sb
        [title] => dxva sig 68744
        [description] => 
        [conversion_status] => DONE
    ),
    ...
    ...
    ...
    [result 40] => Array
    (
        [doc_id] => 1021237
        [access_key] => key-dogbmich9x5iu09kiki
        [title] => onlisp
        [description] => 
        [conversion_status] => PROCESSING
    )
)
*/
````

## Get conversion status of a document
````php
$doc_id = "1021237";
$data = $scribd->getConversionStatus($doc_id); // returns PROCESSING
````


## Get settings (various meta-information) of a document
````php
$doc_id = "1026450";
$data = $scribd->getSettings($doc_id); // returns Array ( [doc_id] => 1021237 [title] => onlisp [description] => [access] => public [license] => by-nc [tags] => [show_ads] => default [access_key] => key-dogbmich9x5iu09kiki )
````

## Changing the settings of a document
````php
$doc_ids = array("1026450"); // This dosen't HAVE to be an array, im simply demonstrating that it can be done with one.
$title = "New, Updated Title 2";
$description = "Updated Description";
$access = "private"; 
$license = "pd"; //public domain.. c for normal copyright.. ect.
$parental_advisory = "adult";
$show_ads = "false"; // setting this to "default" will use the configured option in your account
$tags = "tag, another tag, another tag"; //You can also use an array here
$data = $scribd->changeSettings($doc_ids, $title, $description, $access, $license, $parental_advisory, $show_ads, $tags); //returns 1
````

## Delete a document
````php
$doc_id = "1024559";
$data = $scribd->delete($doc_id); //returns 1
````
## Login as a user
````php
$username = "aeinstein3";
$password = "whitehair";
$data = $scribd->login($username, $password); // Array ( [session_key] => sess-1d9t8wze460fbhp7jw0p [user_id] => 195134 [username] => aeinstein3 [name] => )
````

## Create a new Scribd account
**Note:** you need to login as a user before adding files, signup does not automaticly 'sign you in'
````php
$username = "aeinstein3";
$password = "whitehair";
$email = "ae04@gmail.com";
$name = "Alby Dinosour"; // optional
$data = $scribd->signup($username, $password, $email, $name); //returns Array ( [session_key] => sess-1d9t8wze460fbhp7jw0p [user_id] => 195134 [username] => aeinstein3 [name] => )
````

## Search for docs on Scribd
````php
$query = "fun";
$num_results = 20;
$num_start = 10; // this will bring results 10-30 back
$scope = "all"; // user (default) or all -- using test will throw an error.

try{
    $data = $scribd->search($query, $num_results, $num_start, $scope); // returns
}catch( exception $e){
    $trace = $e->getTrace();
    echo "<br /><b>Scribd Error</b>: ".$e->getMessage()." in <b>".$trace[1]['file']."</b> on line <b>".$trace[1]['line']."</b><br />"; 
}

print_r($data);

/*

Assuming $scope is set to default or all :)

Array
(
    [result] => Array
        (
            [doc_id] => 501796
            [title] => Andrew Loomis - Fun WIth a Pencil
            [description] => 
        )

[.. cut for brevity ..]

    [result 20] => Array
        (
            [doc_id] => 515437
            [title] => Andrew Loomis - Fun With A Pencil
            [description] => 
        )

*/
````
