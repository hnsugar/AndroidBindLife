#Android ����ʵ���������ڰ�

#####��֪�������û�ù�Glide�������ͼƬ�ĵ������⣬�ǳ����ã�Glide��ʵ���˰�Activity��Fragment�������ڵķ������������½��ľ��ǿ���GlideԴ����ܽ�ľ������ݡ�

###1.���ȴ�һ��ʣ�ΪʲôҪ���������ڣ�

####�Ҿټ������ӣ�
Glide��ImageView����Activity����fragment�����ˣ������Imageview��ʹ�õ�ͼ����Դ�Ϳ����ͷ��ˣ�����˵�ǡ��ֶ����ͷ��ˣ���Ȼ�ǻص��ķ�ʽʵ�ֵġ�

  ���о��ǣ���������������һ��ҳ�淢�����������󣬵����ֺܿ�ر��ˣ������������ܾͲ���Ҫ�����ˣ���֮���һᷢһƪ�Լ�дһ���÷���ʵ�ֻ���Ŀ�ܣ�ֻ�в���300�д��룩��һЩ������������֧������ȡ���ģ�����cancel�����������õ�������󶨣���Ȼ�ˣ�Ҳ�����ڷ��ؽ�������ж��Ƿ����������ǡ�������activity�����������ٵģ������ж�����״̬���鷳����ʹ�ð��������ڷ��������Ժ��������������ʹ��дһ����ܣ�ʵ���Զ�ȡ�����������ǲ��Ǹ��ߴ����أ�

�ܽ�һ�£�������������һ���Ż�������ܵ�;��֮һ����ϵͳ�ٴ���һЩû��������飬������ô�û���Ҫ������Ա�����Խ

###2.ԭ��
ԭ��ܼ򵥣����֪��fragment��activity�ϵ����������Ǹ���activity�仯�ģ����ԣ���������Ҫ�󶨵�activity�ϼ���һ����������fragment�Ϳ����ˣ��ǲ��Ǻܼ� (�ޣ���)V��

###3.��ʼд����
####1.����һ���ӿڣ������ص���
	public interface onQK_ILifeListener {
	        public void onStart ();
	        public void onDestroy ();
	        public void onStop ();
	        public void onCreat ();
	        public void onPause ();
	        public void onResume ();
	
	        /**
	         * ʹ���������
	         * @param isHidden
	         */
	        public void onFragmentHiddenChanged (boolean isHidden);
	}
####2.��������Fragment��һģһ��������һ���Ǽ̳�V4���ģ�

	public class QK_ListenerFragment extends Fragment {
	    private onQK_ILifeListener mlistener;
	    public static final String ListenerFragmentTag = "ListenerFragment";

	    public QK_ListenerFragment () {
	
	    }
	
	    public void setLifeListener (onQK_ILifeListener mlistener) {
	        this.mlistener = mlistener;
	    }
	
	    @Override
	    public void onCreate (@Nullable final Bundle savedInstanceState) {
	        super.onCreate (savedInstanceState);
	        if (mlistener != null) {
	            mlistener.onCreat ();
	        }
	    }
	
	    @Override
	    public void onResume () {
	        super.onResume ();
	        if (mlistener != null) {
	            mlistener.onResume ();
	        }
	    }
	
	    @Override
	    public void onHiddenChanged (final boolean hidden) {
	        super.onHiddenChanged (hidden);
	        if (mlistener != null) {
	            mlistener.onFragmentHiddenChanged (hidden);
	        }
	    }
	
	    @Override
	    public void onPause () {
	        if (mlistener != null) {
	            mlistener.onPause ();
	        }
	        super.onPause ();
	    }
	
	    @Override
	    public void onStart () {
	        super.onStart ();
	        if (mlistener != null) {
	            mlistener.onStart ();
	        }
	    }
	
	    @Override
	    public void onStop () {
	        if (mlistener != null) {
	            mlistener.onStop ();
	        }
	        super.onStop ();
	    }
	
	    @Override
	    public void onDestroy () {
	        if (mlistener != null) {
	            mlistener.onDestroy ();
	        }
	        super.onDestroy ();
	    }
	
	}

��һ��android.support.v4.app.Fragment

	public class QK_ListenerFragmentV4 extends android.support.v4.app.Fragment {
	    //�������һģһ����������d=====(������*)b
	}


####3.���������ڣ���ActivityΪ��
	/**
     * ��ϵͳĬ��Activity
     *
     * @param mContext
     * @param mListener
     */
    public static void bindLife (final Activity mContext, onQK_ILifeListener mListener) {
        //�ж��ǲ���ԭ��Activity��������ǡ���������ô���ܣ��Ͻ���b(������)d
        if (mContext instanceof Activity) {
            QK_ListenerFragment listenerFragment = new QK_ListenerFragment ();
            //���ü���
            listenerFragment.setLifeListener (mListener);
            FragmentManager manager = mContext.getFragmentManager ();
            //��������
            manager.beginTransaction ().add (listenerFragment, QK_ListenerFragment.ListenerFragmentTag).commitAllowingStateLoss ();
        }
    }
���ˣ�ֱ�ӵ��þ����ˣ�
	 QKManager.bindLife (this, new onQK_LifeListener () {
	
	            @Override
	            public void onStart () {
	
	                mTextView.append ("\r\n" + "DemoBindLifeActivity.onStart");
	            }
	
	            @Override
	            public void onDestroy () {
	                mTextView.append ("\r\n" + "DemoBindLifeActivity.onDestroy");
	            }
	
	            @Override
	            public void onStop () {
	                mTextView.append ("\r\n" + "DemoBindLifeActivity.onStop");
	            }
	
	            @Override
	            public void onCreat () {
	                mTextView.append ("\r\n" + "DemoBindLifeActivity.onCreat");
	            }
	
	            @Override
	            public void onPause () {
	                mTextView.append ("\r\n" + "DemoBindLifeActivity.onPause");
	            }
	
	            @Override
	            public void onResume () {
	                mTextView.append ("\r\n" + "DemoBindLifeActivity.onResume");
	            }
	
	            @Override
	            public void onFragmentHiddenChanged (final boolean isHidden) {
	                mTextView.append ("\r\n" + "DemoBindLifeActivity.onFragmentHiddenChanged");
	            }
	        });
####4.FragmentActivity��ʽ
	 /**
	     * ��V4���µ�FragmentActivity
	     *
	     * @param mContext
	     * @param mListener
	     */
	    public static void bindLife (final FragmentActivity mContext, onQK_ILifeListener mListener) {
	        if (mContext instanceof FragmentActivity) {
	            QK_ListenerFragmentV4 listenerFragment = new QK_ListenerFragmentV4 ();
	            listenerFragment.setLifeListener (mListener);
	            android.support.v4.app.FragmentManager manager = mContext.getSupportFragmentManager ();
	            manager.beginTransaction ().add (listenerFragment, QK_ListenerFragmentV4.ListenerFragmentTag).commitAllowingStateLoss ();
	        }
	    }
####5.Ĭ��Fragment��,ʹ�õ���FragmentManager ����ס
	    /**
	     * ��ϵͳĬ��Fragment
	     *
	     * @param mContext
	     * @param mListener
	     */
	    public static void bindLife (final Fragment mContext, onQK_ILifeListener mListener) {
	        if (mContext instanceof Fragment) {
	            QK_ListenerFragment listenerFragment = new QK_ListenerFragment ();
	            listenerFragment.setLifeListener (mListener);
	            FragmentManager manager = mContext.getFragmentManager ();
	            manager.beginTransaction ().add (listenerFragment, QK_ListenerFragment.ListenerFragmentTag).commitAllowingStateLoss ();
	        }
	    }
####6.��V4���µ�Fragment ,  android.support.v4.app.Fragment
	    /**
	     * ��V4���µ�Fragment
	     *
	     * @param mContext
	     * @param mListener
	     */
	    public static void bindLife (final android.support.v4.app.Fragment mContext, onQK_ILifeListener mListener) {
	        if (mContext instanceof android.support.v4.app.Fragment) {
	            QK_ListenerFragmentV4 listenerFragment = new QK_ListenerFragmentV4 ();
	            listenerFragment.setLifeListener (mListener);
	            android.support.v4.app.FragmentManager manager = mContext.getFragmentManager ();
	            manager.beginTransaction ().add (listenerFragment, QK_ListenerFragmentV4.ListenerFragmentTag).commitAllowingStateLoss ();
	        }
	    }

####7.�ۺ�һ�£���һ������

	  public static void bindLife (Object mContext, onQK_ILifeListener mListener) {
	        if (mContext instanceof Activity) {
	            QK_ListenerFragment listenerFragment = new QK_ListenerFragment ();
	            listenerFragment.setLifeListener (mListener);
	            FragmentManager manager = ((Activity) mContext).getFragmentManager ();
	               manager.beginTransaction ().add (listenerFragment, QK_ListenerFragment.ListenerFragmentTag).commitAllowingStateLoss ();
	        } else if (mContext instanceof FragmentActivity) {
	            QK_ListenerFragmentV4 listenerFragment = new QK_ListenerFragmentV4 ();
	            listenerFragment.setLifeListener (mListener);
	            android.support.v4.app.FragmentManager manager = ((FragmentActivity) mContext).getSupportFragmentManager ();
	            manager.beginTransaction ().add (listenerFragment, QK_ListenerFragmentV4.ListenerFragmentTag).commitAllowingStateLoss ();
	        } else if (mContext instanceof Fragment) {
	            QK_ListenerFragment listenerFragment = new QK_ListenerFragment ();
	            listenerFragment.setLifeListener (mListener);
	            FragmentManager manager = ((Fragment) mContext).getFragmentManager ();
	            manager.beginTransaction ().add (listenerFragment, QK_ListenerFragment.ListenerFragmentTag).commitAllowingStateLoss ();
	        } else if (mContext instanceof android.support.v4.app.Fragment) {
	            QK_ListenerFragmentV4 listenerFragment = new QK_ListenerFragmentV4 ();
	            listenerFragment.setLifeListener (mListener);
	            android.support.v4.app.FragmentManager manager = ((android.support.v4.app.Fragment) mContext).getFragmentManager ();
	            manager.beginTransaction ().add (listenerFragment, QK_ListenerFragmentV4.ListenerFragmentTag).commitAllowingStateLoss ();
	        }
	    }
####8.��������
��������Ǽ򵥵�ʵ�ְ󶨣����һ��ҳ��󶨺ö���ǲ��Ǿ�������˷��أ����ǿ϶��ģ����һ��Activity����fragmentֻ����һ����ӵ�fragment����Ҫ���жϻ�Ҫ����һ�����⣬��������ˣ�Ŀǰ�ҵĽ������������tag����̬���Ϻ�findFragmentByTag�ȡ���������֤һ��Activity����fragmentֻ����һ�����ǵ�fragment,�����Ƿ���һ�������ÿ�δ����ͱ���һ��ִ�лص������ˣ�˼·���������ˣ�����Լ���չ˼·�ɡ�(o�b���b)o��[BINGO!]



####9.Сβ��
�����ڵ����Լ�����վ��http://lujianchao.com        ����������ʲô��ż�������⣬�������ڵ��ԡ�������
GitHub��https://github.com/hnsugar    
CSDN:http://blog.csdn.net/hnsugar
��Ŀ��ַ�����Լ���Git������  http://lujianchao.com:8081/gitblit/summary/AndroidTips!DemoBindLife.git