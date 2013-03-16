Description
-------------------------

This distribution is based on solr 3.5.0.2011.11.30.16.37.06 with mmseg4j(1.8.5) tokenizer and tika(0.10), which helps to index&search 
chinese, both plaintext and rich format attachments.

Solr example configuration
--------------------------

To run this example configuration, use 

<pre>
  <code>
    java -jar start.jar
  </code>
</pre>

in this directory, and when Solr is started connect to 

  http://localhost:8983/solr/admin/

For ROR useage
--------------------------

There is a [great tutorial](http://goo.gl/BJKUo) introduce how to index rich documents with rails, sunspot and sunspot_cell

For now in your Gemfile, there's no need to add <code> gem 'sunspot_cell_jars' </code>, the tika related jars have already been encapsulated in.

<pre>
  <code>
    gem 'sunspot_rails'

    gem 'sunspot_solr'

    gem 'sunspot_cell', :git => 'git://github.com/zheileman/sunspot_cell.git'
  </code>
</pre>

In the model 

<pre>

  <code>

    class Attachment < ActiveRecord::Base

      mount_uploader :upload, UploadUploader

      # Sunspot indexing configuration
      searchable do
        # Regular fields that I want indexed
        integer :id
        time :created_at
        time :updated_at
        text :file_name
        # For Sunspot Cell. The 'attachment' directive instructs
        # Cell how to get the binary data. My understanding is that
        # this *must* end in _attachment
        attachment :document_attachment
      end
      # Goes hand-in-hand with the item above. Now, this is important:
      # the return value from this method is NOT the binary data itself,
      # but rather the full URI to the file. Cell will use this to locate
      # the file and index it.
      def document_attachment
        "#{Rails.root}/public/#{upload.url}"
      end
      ... ...
    end

  </code>

</pre>

For the detailed sunspot configuration please reference [here](http://goo.gl/H0S1B)

License
--------------------------------------
solr_cn is distributed under the MIT License
