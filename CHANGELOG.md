# 0.0.4
Added prerender status code and header.

When an stateChangeError or a stateNotFound event is thrown, the status is set automatically (to 500 and 404 respectively), but it also offers manual control if needed.

```javascript
function configureRoutes($stateProvider) {
    $stateProvider
        .state('blogposts', {
            url: '/posts/:category',
            resolve: {
                /* @ngInject */
                posts: function(myService, $stateParams) {
                    return myService.getPosts($stateParams.category);
                }
            }
            metaTags: {
                prerender: {
                    /* @ngInject */
                    statusCode: function(posts) {
                        return posts.length > 0 ? 200 : 302;
                    },
                    /* @ngInject */
                    header: function(posts) {
                        return posts.length > 0 ? null : 'Location: http://example.com/posts';
                    }
                }
            }
        });
}
angular
    .module('myApp')
    .config(configureRoutes);
```

In the example above, if there are posts, then a status code of 200 is returned, whereas if no posts are there, then a 302 is set and a redirect location is offered.


# 0.0.3
Added the possibility to have static tags and to set the 'og:url' property by default.

```javascript
function configure(UIRouterMetatagsProvider) {
    UIRouterMetatagsProvider
        .setTitlePrefix('prefix - ')
        .setTitleSuffix(' | MyApp');
        .setDefaultTitle('MyApp')
        .setDefaultDescription('description')
        .setDefaultKeywords('keywords')
        .setStaticProperties({
                'fb:app_id': 'your fb app id',
                'og:site_name': 'your site name'
            })
        .setOGURL(true);
}

angular
    .module('myApp')
    .config(configure);
```

# 0.0.2
Tags can now resolve properties resolved by ui-router

Example:

```javascript
function configureRoutes($stateProvider) {
    $stateProvider
        .state('blogpost', {
            url: '/post/:id',
            resolve: {
                /* @ngInject */
                blogpost: function(myService, $stateParams) {
                    return myService.getPost($stateParams.id);
                }
            }
            metaTags: {
                /* @ngInject */
                title: function(blogpost) {
                    return blogpost.title;
                },
                description: 'The most interresting post {{blogpost.title}}'
            }
        });


}
angular
    .module('myApp')
    .config(configureRoutes);
```

# 0.0.1
Initial version