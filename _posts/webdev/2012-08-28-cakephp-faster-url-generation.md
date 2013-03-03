---
layout: post
category : webdev
tags : [php, cakephp]
---

## CakePHP faster URL generation

CakePHP framework provides a series of helpers to facilitate the creation of views of web systems. But one must note that each time you put a link in your view using a helper method, the Router::url() method is called and the URL string is computed.

The problem is when there is a page with many links. This links might have a common prefix (i.e. targeting the same controller/action). For example, the typical index actions shows a list of rows, and in each row there is a link to edit and delete. Generally this links follow the pattern /:controller/:action/:id. 

The idea here is that is not necessary to compute the part /:controler/:action each time a URL is generated. We could cache this part.

In order to do that, we can rewrite the method url() of the app/AppHelper.php.

	var $urls = array();
	/* increases url performance in 50%, by "caching" urls */
	function url($url = null, $full = false){
		if (
			$full || 
			!is_array($url) || 
			array_diff(
				array_keys($url), 
				array('controller', 'action', 0, 1, 2, 3, 4))){
			return parent::url($url, $full);
		}
		if (!isset($url['controller'])){
			$url['controller'] = $this->params['controller'];
		}
		if (!isset($url['action'])){
			$url['action'] = $this->params['action'];
		}
		$key = $url['controller'].'#'.$url['action'];
		if (!isset($this->urls[$key])){
			$pre = $this->urls[$key] = parent::url(array(
				'controller' => $url['controller'], 
				'action' => $url['action']));
		} else {
			$pre = $this->urls[$key];
		}
		for ($i=0; $i<4; $i++)
			if (isset($url[$i]))
				$pre.=DS.$url[$i];
		return $pre; 
	}