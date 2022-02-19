# Twig
Hello, this is help to get fields in twig file.

-----------------------------------------Webform--------------------------------------

Webform file Naming Convention **webform--webform-file-name.html.twig**
-----------------------------------------------------------------------
Get the **Webform field** in twig file.

        {{ { '#type': 'webform', '#webform': 'machineName' } }}
        
for Text field, Email, Select, Checkbox, Textarea....     

	{{ element.elements.field_machine_name }}

---------------------------------------------------Paragraph------------------------------
        
Paragraph file Naming Convention **paragraph--paragraph-file-name.html.twig**
----------------------------------------------------------------------------
Get the **Paragraph field** in twig file.

for text(plain) field                             {{ content.field_machine_name }}
for viewfield                                     {{ content.field_machine_name }}
for Link field        

	<a href="{{ content.field_machine_name[0]['#url']|render }}">{{ content.field_machine_name[0]['#title']|render }}</a> 
for Image field                                   

	<img src="{{ file_url(paragraph.field_right_branch[0].entity.uri.value) }}">
for Text (formatted, long, with summary) field    {{ content.field_machine_name }}

If we use Paragraph into Paragraph as a Entity reference revisions field for repeated section:-

First Approch:-

     {% for key, item in content["field_entity_reference_revision_name"] %}
                {% if key matches '/^\\d+$/' %}
                
                    {# for Image Field #}                      
     <img src="{{ file_url(item['#paragraph'].machine_name_of_field_image.entity.uri.value) }}">
             <h2>{{ item['#paragraph'].machine_name_of_field_text.value }}</h2>
             <p>{{ item['#paragraph'].machine_name_of_field_text_formatted.value|raw }}</p>
	     {{ item['#paragraph'].field_ig_link.0.url|render }}{{ item['#paragraph'].field_ig_link.0.title|render }}
             
                {% endif %}
		{% endfor %}
    
Second Approch:-

    {% for key, option in paragraph.field_entity_reference_revision_name %}
                {% if key matches '/^\\d+$/' %}
                
                    {# for Image Field #}                      
             <img src="{{ file_url(option.entity.machine_name_of_field_image.entity.fileuri) }}">
                    {# 	Text (plain, long) #}
             <p>{{ option.entity.machine_name_of_field_text.value }}</p>
             
                {% endif %}
		{% endfor %}
    
**Twig Filters
With the Paragraph we use raw filter {{ .value|raw }}

With the content we use striptags filter {{ .value|striptags }}
striptags filter replace adjacent whitespace by one space.

**You can also provide tags which should not be stripped:

	{{ some_html|striptags('<br><p>') }}


----------------------------------------------------------View--------------------------------------------
           
{{ base_path ~ directory }}/for-img-path
  
Mainly 3 Twig files are important for Views.
	
1.)     views-view--display-id.html.twig
                       or
        views-view--display-id--machine-name.html.twig

2.)     views-view-unformatted--display-id.html.twig
                      or
        views-view-unformatted--display-id--machine-name.html.twig
  
3.)     views-view-fields-display-id.html.twig
                      or
        views-view-fields--display-id--machine-name.html.twig
	
------------------------------------------------------------------
  
**  It's come in **views-view--display-id.html.twig**
  
		  <section class="">
		    <div class="container">
		    </div>
		  </section>
  
  it's a outer part of view..
  
**  It's come in **views-view-unformatted--display-id.html.twig**
  
	  {% for row in rows %}
	    {{ row.content }}
	  {% endfor %}
  
  here is mention the repeated body..
  
**  It's come in **views-view-fields--display-id.html.twig**  
  
	  <div class="col-md-4">
		{# this is for title field which is provide by views by default #} 
		{{ fields.title.content }}
	      {{ fields.field_machine_name.content }}
	      <img src="{{ file_url(row._entity.field_headerbild.entity.field_media_image.entity.uri.value) }}">
	  </div>
  
  here we mention the field which is not repeated..
  
  
 Another Approch:--
    If you get the fields in **views-view-unformatted--display-id.html.twig**
    then no need to create the **views-view-fields--display-id.html.twig**
    
	{% for row in rows %}
	 <div class="col-lg-6 col-md-6 col-sm-12">
			<div class="event-card">
			  {% set node_url =  path('entity.node.canonical', {'node': row.content['#row']._entity.get('nid').value}) %}
		<a href="{{ node_url }}">
		      <img class="" src="{{ file_url(row.content['#row']._entity.machine_name_of_field_image.entity.uri.value) }}">
		    <div class="card-body">
			  <p class="event-card-text">{{ row.content['#row']._entity.machine_name_of_field_text_plain[0].value }}</p>
			  {{ row.content['#row']._entity.field_event_date[0].value| date("j M Y") }}
		    </div>
		</a> 
			</div>
		</div>
	{% endfor %}
    
---------------------------------------
For **Node twig**:-
	node--machine-name.html.twig
	
	to get the Title field in node.html.twig
	{{ node.label }}
	
-------------------------------------

** Attaching a library in a **twig template**

		{{ attach_library('your_module/library_name') }}


** Attaching a library in a **preprocess function**

		function yourmodule_preprocess_maintenance_page(&$variables) {
		  $variables['#attached']['library'][] =  'your_module/library_name';
		}
		
		
		function mytheme_preprocess_page(&$variables){
		  if ($variables['is_front'] == TRUE) {
		    $variables['#attached']['library'][] = 'my-theme/my-library';
		  }
		}

             
------------------------------------

Generate relative paths
	using this function  path($name, $parameters, $options) like this:

		{#   path= / #}
		<a href="{{ path('view.frontpage.page_1') }}">{{ 'View all content'|t }}</a>
		{#  path=  /user/1 #}
		<a href="{{ path('entity.user.canonical', {'user': 1}) }}">{{ 'View user'|t }}</a>
		{#   path /node/1 #}
		<a href="{{ path('entity.node.canonical', {'node': 1}) }}">{{ 'View node'|t }}</a>
		
		<a href="{{ path('route_name', {'id': 1}) }}">{{ 'clickable text'|t }}</a>
		Twig
		
Generate absolute  paths
	using this function  url($name, $parameters, $options) like this:

		{# return path like this  http://dev.domaine.com/node/1 #}
		<a href="{{ url('entity.node.canonical', {'node': 1}) }}">{{ 'View all content'|t }}</a>

		{# return path to current page#}
		<a href="{{ url('<current>') }}">{{ 'Reload Page'|t }}</a>

		{#  return path to home page #}
		<a href="{{ url('<front>') }}">{{ 'Home Page'|t }}</a>

