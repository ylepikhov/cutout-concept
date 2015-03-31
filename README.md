# cutout-concept
"Cutout" templating system concept.

This project represents the first take on yet another logicless templating system named "Cutout".
Main idea behind Cutout system, is to enable a programmer to program template logic in his favorite programming language.

Firstly, we takes template prototype, say, simple .html file.

```
<!DOCTYPE html>
<html>
	<head>
		
		<meta charset="UTF-8"/>
		
		<title></title>
		
	</head>

	<body>
	
		<header>
		
			<h1></h1>
			<nav>
				<ul>
					<li>
						<a href=""></a>
					</li>
				</ul>
			</nav>
			
			<span class="warning">session timeout</span>
			<span class="warning">we don't support your client, sorry</span>
			<span class="error">oops...</span>
		</header>
	
		<main>
			
		</main>
		
		<footer>
			
			<small></small>
		</footer>
	</body>
</html>
```

Next, we "cut" it to parts and "mark" injection points.

```
<!DOCTYPE html>
<html>
	<head>
	{?head}
		
		<meta charset="UTF-8"/>
		
		<title>{title}</title>
		
	{/head}
	
	</head>

	<body>
	{?body}
	
		<header>
		
			<h1>{heading}</h1>
			<nav>
			{?navigation}
				<ul>
					{?links}
					<li>
						<a href="{href}">{text}</a>
					</li>
					{/links}
				</ul>
			{/navigation}
			</nav>
			
			{?status}
				{?timeout}<span class="warning">session timeout</span>{/timeout}
				{?client_unsupported}<span class="warning">we don't support your client, sorry</span>{/client_unsupported}
				{?error}<span class="error">oops...</span>{/error}
			{/status}
		</header>
	
		<main>
			{?content}{/content}
		</main>
		
		<footer>
		{?footer}
			
			<small>{copyright}</small>
		{/footer}
		</footer>
	{/body}
	</body>
</html>
```

"Cut" syntax contains open and close tags, like {?head} and {/head} in example above. The "head" string in these tags defines a name of the "cut". Everything between "cut" tags is treated as nested template.

"Mark" syntax contains one tag, like {title} in example above. The "title" string in this tag defines a name of the "mark".
Additionally, "mark" name may be followed by equal sign (=) and default string value.   

After that we generate code for target programming language.

For entire template we get a "template object". For each "template object" there is "renderer function" and "set of parts". 

"Renderer function" accepts parameters (if any) and returns "renderer". We got to provide parameters for each top level (not contained within any "cut") "cut" and "mark" within template. For each top level "mark" we got to provide string value. For each top level "cut" we got to provide string or "renderer" value. 

"Renderer" is a function that renders tamplate. "Renderer" function may accept additional parameters, such as output stream, depending on implementation. 

"Set of parts" consists of "mark parts" and "cut parts".

"Mark part" consists of "mark" name and default string values, if any.

"Cut part" consist of "cut" name and "template object".

"Cut" tags content should be treated as nested template. So "template object" should be generated for each "cut".
