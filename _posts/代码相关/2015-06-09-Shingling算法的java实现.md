---
layout: post
title: Shingling算法的java实现
category: 代码相关
date: 2015-06-09
---

标签： java  Shingling

<!-- more -->

**Shingling java 实现**

    import java.util.ArrayList;
    import java.util.HashMap;
    import java.util.HashSet;
    import java.util.List;
    import java.util.Map;
    import java.util.Set;

    public class Shingling {
    	/*
    	 * 判断hashCodeA与hashCodeB的相似度是否超过给定阈值threshold
    	 */
    	public static boolean isSimilar(Set<String> strA,Set<String> strB, int N, double threshold){

    		double jaccardIndex=jaccardIndex(strA, strB);
    		if(jaccardIndex>threshold){
    			return true;
    		}else{
    			return false;
    		}
    	}

    	/*
    	 * 获取输入字符串inputString的N-Gram字符串列表
    	 */
    	public static List<String> getNGramList(String inputString, int N){
    		List<String> nGramList=new ArrayList<String>();
    		for(int i=0;i<inputString.length()-N+1;i++){
    			nGramList.add(inputString.substring(i, i+N));
    			//转为hash
    //			nGramList.add(HashFunctionLib.PJWHash(inputString.substring(i, i+N))+"");
    		}
    		return nGramList;
    	}

    	/*
    	 * 计算sentence字符串中N-gram的词项数及各词项的权重
    	 */
    	public static Map<String, Integer> getTokenAndWeight(String sentence, int N){
    		List<String> nGramList=getNGramList(sentence, N);
    		Map<String, Integer> resultMap=new HashMap<String, Integer>();
    		for(int i=0;i<nGramList.size();i++){
    			String key=nGramList.get(i);
    			if(!resultMap.containsKey(key)){
    				resultMap.put(key, 1);
    			}
    			else{
    				resultMap.put(key, resultMap.get(key)+1);
    			}
    		}
    		return resultMap;
    	}
    	/*
    	 * A、B:待计算jaccard系数的两个Integer集合
    	 * K:用于计算jaccard系数的哈希值最小的K个元素
    	 * 如果size(A)<K或size(B)<K，则用min(size(A), size(B))作为K的实际值
    	 */
    	public static double jaccardIndex(Set<String> minHashA, Set<String> minHashB){
    		/*
    		 * 计算Jaccard系数
    		 */
    		Set<String> mergedSet=new HashSet<String>();
    		mergedSet.addAll(minHashA);
    		mergedSet.addAll(minHashB);
    		double jaccardIndex=(double)(minHashA.size()+minHashB.size()-mergedSet.size())/(double)mergedSet.size();
    		return jaccardIndex;
    	}
    }

**使用方法：**



 获取文本每4个字为一组的集合

    	List<String> mergedSet =Shingling.getNGramList(text,4);

 在比较两个文本是否相似方法

 看这个两个文本的mergedSet 比较值多少，如果大于某个值确定为相似，这个值根据自己业务来定


   	double bijiaozhi = Shingling.jaccardIndex(mergedSet1，mergedSet2）;





**另个一个简单的比较相似的实现**


   另附（比较短文本的相似度，如果）：



    /**
	 * 两个字符串的相同率
	 */
	public static float RSAC(String str1, String str2) {
		List<String> list = new ArrayList<String>();
		for (int k = 0; k < str1.length(); k++) {
			for (int j = 0; j < str2.length(); j++) {
				if (str1.charAt(k) == str2.charAt(j)
						&& !isExist(list, str1.substring(k, k + 1))) {
					list.add(str1.substring(k, k + 1));
					break;
				}
			}
		}
		float size = (float) (list.size() * 2)
				/ (str1.length() + str2.length());
		DecimalFormat df = new DecimalFormat("0.00");// 格式化小数，不足的补0
		String ratiostr = df.format(size);// 返回的是String类型的
		float ratio = Float.parseFloat(ratiostr);
		return ratio;
	}

	/**
	 * 是否存在相同
	 */
	private static boolean isExist(List<String> list, String dest) {
		for (String s : list) {
			if (dest.equals(s))
				return true;
		}
		return false;
	}




















































