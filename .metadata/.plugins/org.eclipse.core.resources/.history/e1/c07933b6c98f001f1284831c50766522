package com.task.repository;

import java.util.List;

import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

import com.task.entity.Task;
import com.task.entity.TaskMember;

@Repository
public interface TaskMemberRepository extends JpaRepository<TaskMember, Long>{


	List<TaskMember> findByUserIdAndProjectId(Long userId, Long projectId);

	List<TaskMember> findByUserId(Long userId);

	List<TaskMember> findByTaskId(Long id);

	void deleteAllByTeamId(Long teamId);
 
	
	
}
