# samizdat

## self-published markdown, packaged for the distributed web

to read this document directly from IPFS (via a gateway):  [click here](https://gateway.ipfs.io/ipns/samizdat.me/)

what is [markdown](https://guides.github.com/features/mastering-markdown/)?

dead-simple syntax for formatting text. there are headings, lists, links, images, and more. 

* make a new folder
* write markdown in that folder
* add samizdat files
* publish

## samizdat files

download the four files from the following links into your new folder

* [index.md](/index.md) - the markdown
* [index.html](/index.html) - the html
* [markerd.min.js](/marked.min.js) - render the markdown
* [retro.css](/retro.css) - style

edit `index.html` to change the title of your page, or even to swap in a different style. the included style `retro.css` was created by [John Otander](https://markdowncss.github.io/). 

to support multiple pages copy and rename `index.html`and `index.md` accordingly. don't forget to change line 11 of the `html` to match the new `.md` file name.

## publish

we've addressed the 'self' part, now the publishing. there are many ways to get this content in front of readers. the traditional way being to drop these files in a folder served by a webserver. this could be locally on your machine, ftp to a remote machine, even placed in an aws s3 bucket and served as static files. so if this is your outlet to the world go for it. however, I was inspired to create this project by the [inter-planetary file system (ipfs)](https://ipfs.io/).

the tl;dr of ipfs is: `distributed`, `content-addressed` replacement for current concepts of addressing. content-addressed implies that you can ask for a chunk of data (could be a file), and no matter who first created it, anyone who has a copy of it can supply the data. this is where the "inter-planetary" part comes in. our current system of DNS and IP addresses is brittle and centralized. the following [analogy](https://proto.school/content-addressing/01), which I paraphrase, does good work explaining the differences: 

> You would like to get a book recommended by a friend.
> 
> Friend A: go to the Half Price Books in University City, MO, go to the 3rd isle, top shelf, and pick the 11th book from the left.
>
>vs
>
> Friend B: find the book with ISBN# [978-0-521-19533-1](https://www.cs.cornell.edu/home/kleinber/networks-book/)

Friend A is analogous to our current system of DNS and IP addresses. So that if the particular book store closes, is blocked by political boundaries, or even someone moved the book, retrieval of the book will fail. Friend B's addressing allows us to find the nearest copy, or request one from remote gateways (online book stores). 

content-addressing in ipfs is accomplished by hashing the content and using the resulting hash value as the address. in this way, the requestor can be certain that they are retrieving the data they expected by hashing the result and verifying that it matches the address, since changing just one bit of the data would result in a completely different hash value. 

### cli

if you have ipfs installed on your local machine, you can tinker with the mark down and tune things to your preferences quickly. if you have the command line installed, go to the directory with the aforementioned samizdat files and run:

```shell
❯ ipfs add -r .
added QmRJ9qkHWJZ2LeNMeXPsShSo7aZHRn1JNHUK91fx3NXkWC samizdat/index.html
added QmNVSVsd66vNUceAk7Zxwktc9eHYjiXDCiAkPc8e3kC5Yt samizdat/index.md
added QmRetfKYCsEisoBNeZAqhRDxix2cf7FXVBM622hb7K1kxd samizdat/marked.min.js
added Qmb9UHGTsJj4cKc84tjY7i18wrVNqbMH7JWNMweDqFGuwp samizdat/retro.css
added QmQrmokAuv3kfXFGBdFXTJHvpwXuueBPq9PQSf93Gcz6JD samizdat
 47.99 KiB / 47.99 KiB [==========================================] 100.00%
```

the long gnarly strings like `QmQrmokAuv3kfXFGBdFXTJHvpwXuueBPq9PQSf93Gcz6JD` are the addresses of your content. each one represents the constituent files, and the one labeled `samizdat` is the folder containing those files. so if you were to view the contents of that chunk of data, it would look like:

```shell
ipfs object get QmQrmokAuv3kfXFGBdFXTJHvpwXuueBPq9PQSf93Gcz6JD | jq .
{
  "Links": [
    {
      "Name": "index.html",
      "Hash": "QmRJ9qkHWJZ2LeNMeXPsShSo7aZHRn1JNHUK91fx3NXkWC",
      "Size": 949
    },
    {
      "Name": "index.md",
      "Hash": "QmNVSVsd66vNUceAk7Zxwktc9eHYjiXDCiAkPc8e3kC5Yt",
      "Size": 2038
    },
    {
      "Name": "marked.min.js",
      "Hash": "QmRetfKYCsEisoBNeZAqhRDxix2cf7FXVBM622hb7K1kxd",
      "Size": 43889
    },
    {
      "Name": "retro.css",
      "Hash": "Qmb9UHGTsJj4cKc84tjY7i18wrVNqbMH7JWNMweDqFGuwp",
      "Size": 2316
    }
  ],
  "Data": "\b\u0001"
}
```

we see above that the folder is just a series of links(hashes) to the files that live in that folder.

if you are using Brave locally, you should now be able to slap an ipfs protocol specifier on your folder's hash and see the results ipfs://QmQrmokAuv3kfXFGBdFXTJHvpwXuueBPq9PQSf93Gcz6JD.

this also exposes an annoying, but useful feature of content based addressing: it cannot be truly self-referential. in order to add the address of this document to itself, I would need to have every bit of it known, and the address itself can't be known until every bit is present. so by adding that calculated address I'd be changing the document and its address.

to ensure success in the above exercise, run your own experiment and use the hash values you get instead of the ones I use as stand-ins here.

### browser plugin

if you have the [ipfs companion](https://docs.ipfs.io/install/ipfs-companion/) browser plugin installed, you can the icon, then the "my node" button. you'll see a page which has tabs for `status`, `files`, `explore`, `peers`, and `settings`. I encourage you to familiarize yourself with all of these, but our interest today is the `files` tab. once you click through, notice the "+ import" button in the upper right. click this, choose "folder", and select the folder I mentioned earlier where the samizdat files are staged. this will upload them into the IPFS network and show you the hash values, but you can avoid the command line.

### pinata

[pinata.cloud](https://pinata.cloud) is a service that will pin IPFS files as a service. this is a quick and easy way to get started. they have a free tier of up to 1GB of files, and it only takes signing up with an email. this is by far the easiest, most likely to be still working in 3 months, option for the dedicated self-publisher.

once you've created an account, go to the [pinata upload page](https://pinata.cloud/pinataupload), click the "upload directory" button, and upload your samizdat directory. give it a name to help you later distinguish this pin from any others you might upload. now the [pin explorer](https://pinata.cloud/pinexplorer) and you should see your pin, with it's hash address as a clickable link. clicking that link should take you to your samizdat markdown page.

pinata links are actually gateway links, which are a way to access IPFS content from the location-based web.

[https://gateway.pinata.cloud/ipfs/QmVR666qRP814YV7iNtPvvpRwynX2JC67KhJYz4BZFjDcf](https://gateway.pinata.cloud/ipfs/QmVR666qRP814YV7iNtPvvpRwynX2JC67KhJYz4BZFjDcf)

you can use this gateway by swapping out the hash for any other chunk of content. this particular gateway will be certain to have the blocks of data behind your hashes if you've uploaded them via the pinata upload page. however, they are still accessible even if you uploaded your data via cli, browser extension, or something else, but they will likely take a long time to resolve at first. this is due to the work going on behind the scenes to find an IPFS node that holds the requested block. if you're behind a NAT router on your home network, this will also potentially block access from the outside. all of these issues have reasonable solutions, but require time and effort, which is something I hope to minimize with samizdat. the goal is to get you focused on the content you want to publish, rather than the publishing itself.

at first this all seems foreign and strange, but after a few iterations of working on your markdown, uploading it, then viewing it, you'll get the hang of it and it will seem obvious.

## a pointer to the content

another sharp edge of the IPFS world is handling change over time. if you share the above pinata gateway link with someone, and then decide you'd like to add or change the content of the page, that person will not see your changes. since the address references the exact bytes that made up your files when uploaded, that is exactly what they will see. so one option is to just re-share a link to your new hash, the new content.

instead, we'll use a built-in solution for this problem, the [inter-planetary name system (ipns)](https://docs.ipfs.io/concepts/ipns/#example-ipns-setup-with-cli). this allows us to share the ipns link, and then re-point that link to new content as we make changes. first we'll create a private key to sign the ipns record with. anyone who has this private key will have the ability to update this link.

```shell
ipfs key gen samizdat
```

generates a key named `samizdat`. then use that key to sign the hash 

```shell
ipfs name publish --key samizdat /ipfs/QmVR666qRP814YV7iNtPvvpRwynX2JC67KhJYz4BZFjDcf
Published to k51qzi5uqu5djz0b01hwmq0bquwe0ftm3jqt4uvs77h0jxdwgbqo85wjiekl2u: /ipfs/QmVR666qRP814YV7iNtPvvpRwynX2JC67KhJYz4BZFjDcf
```

the output `k51qzi5uqu5djz0b01hwmq0bquwe0ftm3jqt4uvs77h0jxdwgbqo85wjiekl2u` is the ipns hash to share with people. this is also accessible via the gateways, but instead of using the `ipfs` prefix, swap in `ipns`. for example

[https://gateway.pinata.cloud/ipns/k51qzi5uqu5djz0b01hwmq0bquwe0ftm3jqt4uvs77h0jxdwgbqo85wjiekl2u](https://gateway.pinata.cloud/ipns/k51qzi5uqu5djz0b01hwmq0bquwe0ftm3jqt4uvs77h0jxdwgbqo85wjiekl2u)

again, expect the first time you use an ipns record to have a very long wait time (potentially minutes), as the network needs to produce the record linking this hash to its content. just give it time. 

### dnslink

one final step to ease access in our not yet distributed web is to point a domain name at this ipns record so there's a human readable way to access the page. there are detailed instructions available on [dnslink](https://docs.ipfs.io/concepts/dnslink/#dnslink) from the ipfs website. I used [samizdat.me](https://samizdat.me) to point to this page.

## conclusion

> We live together, we act on, and react to, one another; but always and in all circumstances we are by ourselves. The martyrs go hand in hand into the arena; they are crucified alone. Embraced, the lovers desperately try to fuse their insulated ecstasies into a single self-transcendence; in vain. By its very nature every embodied spirit is doomed to suffer and enjoy in solitude. Sensations, feelings, insights, fancies—all these are private and, except through symbols and at second hand, incommunicable. We can pool information about experiences, but never the experiences themselves. From family to nation, every human group is a society of island universes.
>
> Aldous Huxley, The Doors of Perception

the only antidote to the loneliness of living in a society of island universes is building something. write about an experience, explain an idea, teach people who to do something useful

Rob Long - [plantimals.org](https://plantimals.org)