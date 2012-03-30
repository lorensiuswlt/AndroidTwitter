AndroidTwitter
==============

AndroidTwitter is a small android library to let you connect to Twitter and access all it's public API (tweet status, retweet, etc).

How to Use
==========
Here is a sample activity (__TestConnect.java__) to show how to connect to Twitter then save the access token. The access token then can be used later to access Twitter's public API (see [TestPost.java](https://github.com/lorensiuswlt/AndroidTwitter/blob/master/src/net/londatiga/android/TestPost.java))

	public class TestConnect extends Activity {
		private TwitterApp mTwitter;
		private CheckBox mTwitterBtn;

		private static final String twitter_consumer_key = "xxx";
		private static final String twitter_secret_key = "xxxx";

		@Override
		public void onCreate(Bundle savedInstanceState) {
			super.onCreate(savedInstanceState);

			setContentView(R.layout.main);

			mTwitterBtn	= (CheckBox) findViewById(R.id.twitterCheck);

			mTwitterBtn.setOnClickListener(new OnClickListener() {
				@Override
				public void onClick(View v) {
					onTwitterClick();
				}
			});

			mTwitter 	= new TwitterApp(this, twitter_consumer_key,twitter_secret_key);

			mTwitter.setListener(mTwLoginDialogListener);

			if (mTwitter.hasAccessToken()) {
				mTwitterBtn.setChecked(true);

				String username = mTwitter.getUsername();
				username		= (username.equals("")) ? "Unknown" : username;

				mTwitterBtn.setText("  Twitter (" + username + ")");
				mTwitterBtn.setTextColor(Color.WHITE);
			}

			Button goBtn = (Button) findViewById(R.id.button1);

			goBtn.setOnClickListener(new OnClickListener() {
				@Override
				public void onClick(View v) {
					startActivity(new Intent(TestConnect.this, TestPost.class));
				}
			});
		}

		private void onTwitterClick() {
			if (mTwitter.hasAccessToken()) {
				final AlertDialog.Builder builder = new AlertDialog.Builder(this);

				builder.setMessage("Delete current Twitter connection?")
				       .setCancelable(false)
				       .setPositiveButton("Yes", new DialogInterface.OnClickListener() {
				           public void onClick(DialogInterface dialog, int id) {
				        	   mTwitter.resetAccessToken();

				        	   mTwitterBtn.setChecked(false);
				        	   mTwitterBtn.setText("  Twitter (Not connected)");
				        	   mTwitterBtn.setTextColor(Color.GRAY);
				           }
				       })
				       .setNegativeButton("No", new DialogInterface.OnClickListener() {
				           public void onClick(DialogInterface dialog, int id) {
				                dialog.cancel();

				                mTwitterBtn.setChecked(true);
				           }
				       });
				final AlertDialog alert = builder.create();

				alert.show();
			} else {
				mTwitterBtn.setChecked(false);

				mTwitter.authorize();
			}
		}

		private final TwDialogListener mTwLoginDialogListener = new TwDialogListener() {
			@Override
			public void onComplete(String value) {
				String username = mTwitter.getUsername();
				username		= (username.equals("")) ? "No Name" : username;

				mTwitterBtn.setText("  Twitter  (" + username + ")");
				mTwitterBtn.setChecked(true);
				mTwitterBtn.setTextColor(Color.WHITE);

				Toast.makeText(TestConnect.this, "Connected to Twitter as " + username, Toast.LENGTH_LONG).show();
			}

			@Override
			public void onError(String value) {
				mTwitterBtn.setChecked(false);

				Toast.makeText(TestConnect.this, "Twitter connection failed", Toast.LENGTH_LONG).show();
			}
		};
	}

**This library requires external jar files from Twitter4J and Oauth-signpost, see http://www.londatiga.net/it/how-to-post-twitter-status-from-android/ for more information.**

![Example Image](http://londatiga.net/images/androidtwitter/android_twitter_1.jpg)  ![Example Image](http://londatiga.net/images/androidtwitter/android_twitter_2.jpg) 

Developed By
============

* Lorensius W. L. T - <lorenz@londatiga.net>

Changes
=======

See [CHANGELOG](https://github.com/lorensiuswlt/AndroidTwitter/blob/master/CHANGELOG.md) for details

License
=======

    Copyright 2011 Lorensius W. L. T

    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.